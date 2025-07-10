# Triggers in Firebird

Triggers are special stored procedures that automatically execute in response to specific database events. They are powerful tools for enforcing business rules, maintaining data integrity, and automating database tasks.

## Types of Triggers

Firebird supports several types of triggers:

1. **DML Triggers**: Fire on INSERT, UPDATE, or DELETE operations
2. **Database Triggers**: Fire on database-level events (CONNECT, DISCONNECT, etc.)
3. **DDL Triggers**: Fire on DDL operations (Firebird 3.0+)

## Basic Syntax

```sql
CREATE [OR ALTER] TRIGGER trigger_name
FOR table_name
[ACTIVE | INACTIVE]
{BEFORE | AFTER} {INSERT | UPDATE | DELETE}
[POSITION number]
AS
[DECLARE VARIABLE variable_name datatype;]
BEGIN
    /* Trigger body */
END
```

## DML Triggers

### BEFORE INSERT Trigger

```sql
CREATE TRIGGER TRG_EMPLOYEE_BI 
FOR EMPLOYEES
ACTIVE BEFORE INSERT
POSITION 0
AS
BEGIN
    -- Auto-generate ID if not provided
    IF (NEW.EMP_ID IS NULL) THEN
        NEW.EMP_ID = GEN_ID(GEN_EMPLOYEE_ID, 1);
    
    -- Set creation timestamp
    NEW.CREATED_AT = CURRENT_TIMESTAMP;
    
    -- Validate data
    IF (NEW.SALARY < 0) THEN
        EXCEPTION EX_INVALID_SALARY 'Salary cannot be negative';
END
```

### AFTER INSERT Trigger

```sql
CREATE TRIGGER TRG_ORDER_AI
FOR ORDERS
ACTIVE AFTER INSERT
POSITION 0
AS
BEGIN
    -- Update customer statistics
    UPDATE CUSTOMERS
    SET TOTAL_ORDERS = TOTAL_ORDERS + 1,
        LAST_ORDER_DATE = CURRENT_DATE
    WHERE CUSTOMER_ID = NEW.CUSTOMER_ID;
    
    -- Log the order
    INSERT INTO ORDER_HISTORY (
        ORDER_ID, 
        CUSTOMER_ID, 
        ORDER_DATE, 
        TOTAL_AMOUNT,
        ACTION,
        ACTION_DATE
    ) VALUES (
        NEW.ORDER_ID,
        NEW.CUSTOMER_ID,
        NEW.ORDER_DATE,
        NEW.TOTAL_AMOUNT,
        'INSERT',
        CURRENT_TIMESTAMP
    );
END
```

### BEFORE UPDATE Trigger

```sql
CREATE TRIGGER TRG_PRODUCT_BU
FOR PRODUCTS
ACTIVE BEFORE UPDATE
POSITION 0
AS
BEGIN
    -- Track price changes
    IF (OLD.PRICE <> NEW.PRICE) THEN
    BEGIN
        INSERT INTO PRICE_HISTORY (
            PRODUCT_ID,
            OLD_PRICE,
            NEW_PRICE,
            CHANGED_BY,
            CHANGE_DATE
        ) VALUES (
            NEW.PRODUCT_ID,
            OLD.PRICE,
            NEW.PRICE,
            CURRENT_USER,
            CURRENT_TIMESTAMP
        );
    END
    
    -- Update last modified timestamp
    NEW.LAST_MODIFIED = CURRENT_TIMESTAMP;
    
    -- Prevent negative stock
    IF (NEW.STOCK_QUANTITY < 0) THEN
        EXCEPTION EX_NEGATIVE_STOCK 'Stock cannot be negative';
END
```

### BEFORE DELETE Trigger

```sql
CREATE TRIGGER TRG_EMPLOYEE_BD
FOR EMPLOYEES
ACTIVE BEFORE DELETE
POSITION 0
AS
BEGIN
    -- Archive employee data before deletion
    INSERT INTO EMPLOYEES_ARCHIVE
    SELECT OLD.* FROM RDB$DATABASE;
    
    -- Check for dependencies
    IF (EXISTS(SELECT 1 FROM ORDERS WHERE EMPLOYEE_ID = OLD.EMP_ID)) THEN
        EXCEPTION EX_HAS_ORDERS 'Cannot delete employee with existing orders';
END
```

## Multi-Event Triggers

Triggers can handle multiple events:

```sql
CREATE TRIGGER TRG_AUDIT_ALL
FOR CUSTOMERS
ACTIVE AFTER INSERT OR UPDATE OR DELETE
POSITION 0
AS
DECLARE VARIABLE ACTION VARCHAR(10);
BEGIN
    -- Determine the action
    IF (INSERTING) THEN
        ACTION = 'INSERT';
    ELSE IF (UPDATING) THEN
        ACTION = 'UPDATE';
    ELSE IF (DELETING) THEN
        ACTION = 'DELETE';
    
    -- Log the action
    INSERT INTO AUDIT_LOG (
        TABLE_NAME,
        RECORD_ID,
        ACTION,
        USER_NAME,
        ACTION_TIME,
        OLD_DATA,
        NEW_DATA
    ) VALUES (
        'CUSTOMERS',
        COALESCE(NEW.CUSTOMER_ID, OLD.CUSTOMER_ID),
        :ACTION,
        CURRENT_USER,
        CURRENT_TIMESTAMP,
        CASE WHEN DELETING OR UPDATING THEN OLD.CUSTOMER_NAME END,
        CASE WHEN INSERTING OR UPDATING THEN NEW.CUSTOMER_NAME END
    );
END
```

## Context Variables

Firebird provides special context variables in triggers:

- `NEW.column_name`: New value (INSERT, UPDATE)
- `OLD.column_name`: Old value (UPDATE, DELETE)
- `INSERTING`: TRUE if INSERT operation
- `UPDATING`: TRUE if UPDATE operation
- `DELETING`: TRUE if DELETE operation

Example using context variables:

```sql
CREATE TRIGGER TRG_INVENTORY_CHECK
FOR INVENTORY
ACTIVE BEFORE UPDATE
AS
BEGIN
    -- Only check when quantity changes
    IF (UPDATING AND OLD.QUANTITY <> NEW.QUANTITY) THEN
    BEGIN
        -- Log quantity changes
        INSERT INTO INVENTORY_LOG (
            PRODUCT_ID,
            OLD_QTY,
            NEW_QTY,
            CHANGE_QTY,
            CHANGED_BY,
            CHANGE_DATE
        ) VALUES (
            NEW.PRODUCT_ID,
            OLD.QUANTITY,
            NEW.QUANTITY,
            NEW.QUANTITY - OLD.QUANTITY,
            CURRENT_USER,
            CURRENT_TIMESTAMP
        );
        
        -- Alert on low stock
        IF (NEW.QUANTITY < 10 AND OLD.QUANTITY >= 10) THEN
            POST_EVENT 'LOW_STOCK_ALERT';
    END
END
```

## Database Triggers

### ON CONNECT Trigger

```sql
CREATE TRIGGER TRG_DB_CONNECT
ON CONNECT
POSITION 0
AS
BEGIN
    -- Log connections
    INSERT INTO CONNECTION_LOG (
        USER_NAME,
        CONNECT_TIME,
        CLIENT_ADDRESS,
        CLIENT_PROCESS
    ) VALUES (
        CURRENT_USER,
        CURRENT_TIMESTAMP,
        RDB$GET_CONTEXT('SYSTEM', 'CLIENT_ADDRESS'),
        RDB$GET_CONTEXT('SYSTEM', 'CLIENT_PROCESS')
    );
    
    -- Check maintenance window
    IF (CURRENT_TIME BETWEEN TIME '22:00:00' AND TIME '23:59:59') THEN
    BEGIN
        IF (CURRENT_USER <> 'SYSDBA') THEN
            EXCEPTION EX_MAINTENANCE 'Database maintenance in progress';
    END
END
```

### ON DISCONNECT Trigger

```sql
CREATE TRIGGER TRG_DB_DISCONNECT
ON DISCONNECT
POSITION 0
AS
DECLARE VARIABLE SESSION_ID INTEGER;
BEGIN
    -- Update session end time
    SELECT SESSION_ID FROM CONNECTION_LOG
    WHERE USER_NAME = CURRENT_USER
      AND DISCONNECT_TIME IS NULL
    ORDER BY CONNECT_TIME DESC
    ROWS 1
    INTO :SESSION_ID;
    
    IF (SESSION_ID IS NOT NULL) THEN
        UPDATE CONNECTION_LOG
        SET DISCONNECT_TIME = CURRENT_TIMESTAMP
        WHERE SESSION_ID = :SESSION_ID;
END
```

### TRANSACTION Triggers

```sql
CREATE TRIGGER TRG_TRANS_START
ON TRANSACTION START
POSITION 0
AS
BEGIN
    -- Set transaction context
    RDB$SET_CONTEXT('USER_TRANSACTION', 'START_TIME', 
                    CAST(CURRENT_TIMESTAMP AS VARCHAR(50)));
END

CREATE TRIGGER TRG_TRANS_COMMIT
ON TRANSACTION COMMIT
POSITION 0
AS
DECLARE VARIABLE START_TIME TIMESTAMP;
BEGIN
    START_TIME = CAST(
        RDB$GET_CONTEXT('USER_TRANSACTION', 'START_TIME') 
        AS TIMESTAMP
    );
    
    -- Log long transactions
    IF (CURRENT_TIMESTAMP - START_TIME > 0.00347) THEN  -- > 5 minutes
    BEGIN
        INSERT INTO LONG_TRANSACTION_LOG (
            USER_NAME,
            START_TIME,
            END_TIME,
            DURATION
        ) VALUES (
            CURRENT_USER,
            :START_TIME,
            CURRENT_TIMESTAMP,
            CURRENT_TIMESTAMP - :START_TIME
        );
    END
END
```

## DDL Triggers (Firebird 3.0+)

```sql
CREATE TRIGGER TRG_DDL_LOG
BEFORE ANY DDL STATEMENT
POSITION 0
AS
DECLARE VARIABLE DDL_EVENT VARCHAR(50);
DECLARE VARIABLE OBJECT_NAME VARCHAR(50);
BEGIN
    DDL_EVENT = RDB$GET_CONTEXT('DDL_TRIGGER', 'DDL_EVENT');
    OBJECT_NAME = RDB$GET_CONTEXT('DDL_TRIGGER', 'OBJECT_NAME');
    
    -- Log DDL changes
    INSERT INTO DDL_LOG (
        DDL_EVENT,
        OBJECT_NAME,
        USER_NAME,
        EVENT_TIME,
        SQL_TEXT
    ) VALUES (
        :DDL_EVENT,
        :OBJECT_NAME,
        CURRENT_USER,
        CURRENT_TIMESTAMP,
        RDB$GET_CONTEXT('DDL_TRIGGER', 'SQL_TEXT')
    );
    
    -- Prevent certain operations
    IF (DDL_EVENT = 'DROP TABLE' AND CURRENT_USER <> 'SYSDBA') THEN
        EXCEPTION EX_NO_DROP 'Only SYSDBA can drop tables';
END
```

## Managing Triggers

### View Triggers

```sql
-- List all triggers
SELECT 
    RDB$TRIGGER_NAME AS TRIGGER_NAME,
    RDB$RELATION_NAME AS TABLE_NAME,
    CASE RDB$TRIGGER_TYPE
        WHEN 1 THEN 'BEFORE INSERT'
        WHEN 2 THEN 'AFTER INSERT'
        WHEN 3 THEN 'BEFORE UPDATE'
        WHEN 4 THEN 'AFTER UPDATE'
        WHEN 5 THEN 'BEFORE DELETE'
        WHEN 6 THEN 'AFTER DELETE'
    END AS TRIGGER_TYPE,
    RDB$TRIGGER_INACTIVE AS IS_INACTIVE
FROM RDB$TRIGGERS
WHERE RDB$SYSTEM_FLAG = 0
ORDER BY RDB$RELATION_NAME, RDB$TRIGGER_SEQUENCE;

-- View trigger source
SELECT RDB$TRIGGER_SOURCE
FROM RDB$TRIGGERS
WHERE RDB$TRIGGER_NAME = 'TRG_EMPLOYEE_BI';
```

### Enable/Disable Triggers

```sql
-- Disable trigger
ALTER TRIGGER TRG_EMPLOYEE_BI INACTIVE;

-- Enable trigger
ALTER TRIGGER TRG_EMPLOYEE_BI ACTIVE;

-- Disable all triggers on a table
ALTER TABLE EMPLOYEES DISABLE TRIGGER ALL;

-- Enable all triggers on a table
ALTER TABLE EMPLOYEES ENABLE TRIGGER ALL;
```

### Drop Trigger

```sql
DROP TRIGGER trigger_name;
```

## Common Trigger Patterns

### 1. Auto-increment Primary Key

```sql
-- First create a generator
CREATE GENERATOR GEN_CUSTOMER_ID;

-- Then create the trigger
CREATE TRIGGER TRG_CUSTOMER_BI FOR CUSTOMERS
ACTIVE BEFORE INSERT POSITION 0
AS
BEGIN
    IF (NEW.CUSTOMER_ID IS NULL) THEN
        NEW.CUSTOMER_ID = GEN_ID(GEN_CUSTOMER_ID, 1);
END
```

### 2. Maintain Summary Tables

```sql
CREATE TRIGGER TRG_ORDER_DETAIL_AIU FOR ORDER_DETAILS
ACTIVE AFTER INSERT OR UPDATE OR DELETE
AS
DECLARE VARIABLE TOTAL NUMERIC(15,2);
BEGIN
    -- Recalculate order total
    SELECT SUM(QUANTITY * UNIT_PRICE)
    FROM ORDER_DETAILS
    WHERE ORDER_ID = COALESCE(NEW.ORDER_ID, OLD.ORDER_ID)
    INTO :TOTAL;
    
    UPDATE ORDERS
    SET TOTAL_AMOUNT = :TOTAL
    WHERE ORDER_ID = COALESCE(NEW.ORDER_ID, OLD.ORDER_ID);
END
```

### 3. Cascade Operations

```sql
CREATE TRIGGER TRG_DEPARTMENT_BD FOR DEPARTMENTS
ACTIVE BEFORE DELETE POSITION 0
AS
BEGIN
    -- Reassign employees to default department
    UPDATE EMPLOYEES
    SET DEPT_ID = 0  -- Default department
    WHERE DEPT_ID = OLD.DEPT_ID;
    
    -- Delete related records
    DELETE FROM DEPT_PROJECTS
    WHERE DEPT_ID = OLD.DEPT_ID;
END
```

### 4. Data Validation

```sql
CREATE TRIGGER TRG_EMAIL_VALIDATION FOR USERS
ACTIVE BEFORE INSERT OR UPDATE POSITION 0
AS
BEGIN
    -- Validate email format
    IF (NEW.EMAIL NOT LIKE '%@%.%') THEN
        EXCEPTION EX_INVALID_EMAIL 'Invalid email format';
    
    -- Check for duplicate email
    IF (EXISTS(
        SELECT 1 FROM USERS 
        WHERE EMAIL = NEW.EMAIL 
          AND USER_ID <> COALESCE(NEW.USER_ID, 0)
    )) THEN
        EXCEPTION EX_DUPLICATE_EMAIL 'Email already exists';
END
```

## Best Practices

1. **Keep triggers simple**: Complex logic should be in stored procedures
2. **Use appropriate timing**: BEFORE for validation, AFTER for cascading
3. **Handle NULL values**: Always check for NULL in NEW/OLD values
4. **Avoid recursive triggers**: Be careful with updates to the same table
5. **Use meaningful names**: Prefix with TRG_ and include table name
6. **Document trigger logic**: Comment complex business rules
7. **Test thoroughly**: Include edge cases and concurrent access

## Performance Considerations

1. **Minimize trigger overhead**: Avoid complex queries in triggers
2. **Use indexes**: Ensure queries in triggers use indexes
3. **Batch operations**: Consider impact on bulk operations
4. **Monitor execution time**: Long-running triggers affect performance

## Debugging Triggers

```sql
-- Create debug log table
CREATE TABLE DEBUG_LOG (
    LOG_ID INTEGER GENERATED BY DEFAULT AS IDENTITY,
    LOG_TIME TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    TRIGGER_NAME VARCHAR(50),
    MESSAGE VARCHAR(1000)
);

-- Debug trigger example
CREATE TRIGGER TRG_DEBUG_EXAMPLE FOR ORDERS
ACTIVE BEFORE INSERT POSITION 0
AS
BEGIN
    -- Log debug info
    INSERT INTO DEBUG_LOG (TRIGGER_NAME, MESSAGE)
    VALUES ('TRG_DEBUG_EXAMPLE', 
            'Order ID: ' || COALESCE(NEW.ORDER_ID, 'NULL') ||
            ', Customer: ' || COALESCE(NEW.CUSTOMER_ID, 'NULL'));
    
    -- Your trigger logic here
END
```

## Next Steps

- Learn about exception handling in triggers
- Explore trigger interaction with stored procedures
- Study performance optimization for triggers
- Practice implementing complex business rules with triggers