# DB2 Triggers

A trigger is a database object that automatically executes a set of SQL statements when a specific event occurs on a table. DB2 supports triggers for INSERT, UPDATE, and DELETE operations.

## Creating Triggers in DB2

### Basic Syntax

```sql
CREATE TRIGGER trigger_name
  trigger_time trigger_event
  ON table_name
  REFERENCING reference_clause
  FOR EACH {ROW | STATEMENT}
  MODE DB2SQL
  WHEN (condition)
  trigger_body
```

Where:
- `trigger_time`: BEFORE, AFTER, or INSTEAD OF
- `trigger_event`: INSERT, UPDATE, DELETE, or combinations
- `reference_clause`: NEW AS alias, OLD AS alias
- `FOR EACH ROW`: Row-level trigger (executes for each affected row)
- `FOR EACH STATEMENT`: Statement-level trigger (executes once per SQL statement)

## BEFORE Triggers

BEFORE triggers execute before the triggering statement. They can modify NEW values in INSERT and UPDATE operations.

### Example: BEFORE INSERT Trigger

```sql
-- Create sample tables
CREATE TABLE EMPLOYEES (
    EMP_ID INTEGER NOT NULL PRIMARY KEY,
    EMP_NAME VARCHAR(100),
    SALARY DECIMAL(10,2),
    CREATED_DATE TIMESTAMP,
    CREATED_BY VARCHAR(50)
);

CREATE TABLE AUDIT_LOG (
    LOG_ID INTEGER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    TABLE_NAME VARCHAR(50),
    ACTION VARCHAR(10),
    USER_NAME VARCHAR(50),
    ACTION_TIME TIMESTAMP,
    DETAILS VARCHAR(500)
);

-- Create BEFORE INSERT trigger
CREATE TRIGGER EMP_BEFORE_INSERT
  BEFORE INSERT ON EMPLOYEES
  REFERENCING NEW AS N
  FOR EACH ROW
  MODE DB2SQL
  BEGIN
    -- Set creation timestamp if not provided
    IF N.CREATED_DATE IS NULL THEN
      SET N.CREATED_DATE = CURRENT TIMESTAMP;
    END IF;
    
    -- Set created by user
    SET N.CREATED_BY = CURRENT USER;
    
    -- Validate salary
    IF N.SALARY < 0 THEN
      SIGNAL SQLSTATE '75001' 
        SET MESSAGE_TEXT = 'Salary cannot be negative';
    END IF;
  END
```

### Example: BEFORE UPDATE Trigger

```sql
CREATE TABLE EMPLOYEE_HISTORY (
    HISTORY_ID INTEGER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    EMP_ID INTEGER,
    OLD_SALARY DECIMAL(10,2),
    NEW_SALARY DECIMAL(10,2),
    CHANGE_DATE TIMESTAMP,
    CHANGED_BY VARCHAR(50)
);

CREATE TRIGGER EMP_BEFORE_UPDATE
  BEFORE UPDATE OF SALARY ON EMPLOYEES
  REFERENCING OLD AS O NEW AS N
  FOR EACH ROW
  MODE DB2SQL
  WHEN (O.SALARY <> N.SALARY)
  BEGIN
    -- Prevent salary decrease of more than 10%
    IF N.SALARY < O.SALARY * 0.9 THEN
      SIGNAL SQLSTATE '75002'
        SET MESSAGE_TEXT = 'Salary cannot be reduced by more than 10%';
    END IF;
  END
```

## AFTER Triggers

AFTER triggers execute after the triggering statement completes successfully. They cannot modify NEW values but are useful for auditing and logging.

### Example: AFTER INSERT Trigger with Automatic Order History

```sql
-- Create tables for order management
CREATE TABLE ORDERS (
    ORDERID INTEGER NOT NULL PRIMARY KEY,
    CUSTOMERID VARCHAR(50),
    ITEM VARCHAR(50),
    BILL_AMOUNT DECIMAL(10,2),
    ORDER_STATUS VARCHAR(20) DEFAULT 'PENDING'
);

CREATE TABLE ORDER_HISTORY (
    HISTORY_ID INTEGER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    ORDERID INTEGER,
    ITEM VARCHAR(50),
    BILL_AMOUNT DECIMAL(10,2),
    DATE_OF_ORDER TIMESTAMP,
    ACTION VARCHAR(20)
);

-- Create AFTER INSERT trigger
CREATE TRIGGER ORD_AFTER_INSERT
  AFTER INSERT ON ORDERS
  REFERENCING NEW AS N
  FOR EACH ROW
  MODE DB2SQL
  BEGIN
    -- Insert into order history
    INSERT INTO ORDER_HISTORY (ORDERID, ITEM, BILL_AMOUNT, DATE_OF_ORDER, ACTION)
    VALUES (N.ORDERID, N.ITEM, N.BILL_AMOUNT, CURRENT TIMESTAMP, 'ORDER_CREATED');
    
    -- Log the action
    INSERT INTO AUDIT_LOG (TABLE_NAME, ACTION, USER_NAME, ACTION_TIME, DETAILS)
    VALUES ('ORDERS', 'INSERT', CURRENT USER, CURRENT TIMESTAMP, 
            'Order ID: ' || CAST(N.ORDERID AS VARCHAR(20)));
  END
```

### Example: AFTER UPDATE Trigger

```sql
CREATE TRIGGER ORD_AFTER_UPDATE
  AFTER UPDATE ON ORDERS
  REFERENCING OLD AS O NEW AS N
  FOR EACH ROW
  MODE DB2SQL
  WHEN (O.ORDER_STATUS <> N.ORDER_STATUS)
  BEGIN
    -- Track status changes
    INSERT INTO ORDER_HISTORY (ORDERID, ITEM, BILL_AMOUNT, DATE_OF_ORDER, ACTION)
    VALUES (N.ORDERID, N.ITEM, N.BILL_AMOUNT, CURRENT TIMESTAMP, 
            'STATUS_CHANGED_TO_' || N.ORDER_STATUS);
  END
```

### Example: AFTER DELETE Trigger

```sql
CREATE TRIGGER EMP_AFTER_DELETE
  AFTER DELETE ON EMPLOYEES
  REFERENCING OLD AS O
  FOR EACH ROW
  MODE DB2SQL
  BEGIN
    -- Archive deleted employee data
    INSERT INTO AUDIT_LOG (TABLE_NAME, ACTION, USER_NAME, ACTION_TIME, DETAILS)
    VALUES ('EMPLOYEES', 'DELETE', CURRENT USER, CURRENT TIMESTAMP,
            'Deleted Employee: ' || O.EMP_NAME || ', ID: ' || CAST(O.EMP_ID AS VARCHAR(20)));
  END
```

## INSTEAD OF Triggers (for Views)

INSTEAD OF triggers are used with views to make them updatable.

```sql
-- Create a view
CREATE VIEW ACTIVE_EMPLOYEES AS
  SELECT EMP_ID, EMP_NAME, SALARY
  FROM EMPLOYEES
  WHERE CREATED_DATE >= CURRENT DATE - 365 DAYS;

-- Create INSTEAD OF trigger for the view
CREATE TRIGGER ACTIVE_EMP_INSERT
  INSTEAD OF INSERT ON ACTIVE_EMPLOYEES
  REFERENCING NEW AS N
  FOR EACH ROW
  MODE DB2SQL
  BEGIN
    INSERT INTO EMPLOYEES (EMP_ID, EMP_NAME, SALARY, CREATED_DATE, CREATED_BY)
    VALUES (N.EMP_ID, N.EMP_NAME, N.SALARY, CURRENT TIMESTAMP, CURRENT USER);
  END
```

## Statement-Level Triggers

Statement-level triggers execute once per SQL statement, regardless of how many rows are affected.

```sql
CREATE TRIGGER ORDERS_STATEMENT_AUDIT
  AFTER INSERT ON ORDERS
  FOR EACH STATEMENT
  MODE DB2SQL
  BEGIN
    INSERT INTO AUDIT_LOG (TABLE_NAME, ACTION, USER_NAME, ACTION_TIME, DETAILS)
    VALUES ('ORDERS', 'BULK_INSERT', CURRENT USER, CURRENT TIMESTAMP,
            'Statement-level insert operation completed');
  END
```

## Multiple Triggers per Event

DB2 allows multiple triggers for the same event. You can control their execution order using the ORDER clause.

```sql
-- First trigger
CREATE TRIGGER ORDER_VALIDATION_1
  BEFORE INSERT ON ORDERS
  REFERENCING NEW AS N
  FOR EACH ROW
  MODE DB2SQL
  ORDER 1
  BEGIN
    IF N.BILL_AMOUNT <= 0 THEN
      SIGNAL SQLSTATE '75003'
        SET MESSAGE_TEXT = 'Bill amount must be positive';
    END IF;
  END;

-- Second trigger (executes after the first)
CREATE TRIGGER ORDER_VALIDATION_2
  BEFORE INSERT ON ORDERS
  REFERENCING NEW AS N
  FOR EACH ROW
  MODE DB2SQL
  ORDER 2
  BEGIN
    IF N.CUSTOMERID IS NULL THEN
      SIGNAL SQLSTATE '75004'
        SET MESSAGE_TEXT = 'Customer ID is required';
    END IF;
  END
```

## Compound Triggers

DB2 supports compound SQL in triggers for complex logic.

```sql
CREATE TRIGGER COMPLEX_ORDER_PROCESSING
  AFTER INSERT ON ORDERS
  REFERENCING NEW AS N
  FOR EACH ROW
  MODE DB2SQL
  BEGIN
    DECLARE v_customer_total DECIMAL(10,2);
    DECLARE v_discount DECIMAL(5,2) DEFAULT 0;
    
    -- Calculate customer's total orders
    SELECT COALESCE(SUM(BILL_AMOUNT), 0) INTO v_customer_total
    FROM ORDERS
    WHERE CUSTOMERID = N.CUSTOMERID;
    
    -- Apply discount based on total
    IF v_customer_total > 10000 THEN
      SET v_discount = 10;
    ELSEIF v_customer_total > 5000 THEN
      SET v_discount = 5;
    END IF;
    
    -- Update order with discount if applicable
    IF v_discount > 0 THEN
      UPDATE ORDERS
      SET BILL_AMOUNT = BILL_AMOUNT * (1 - v_discount/100)
      WHERE ORDERID = N.ORDERID;
    END IF;
  END
```

## Trigger Management

### Viewing Triggers

```sql
-- List all triggers
SELECT TRIGNAME, TABNAME, TRIGTIME, TRIGEVENT, GRANULARITY
FROM SYSCAT.TRIGGERS
WHERE TRIGSCHEMA = CURRENT SCHEMA;

-- View trigger definition
SELECT TEXT 
FROM SYSCAT.TRIGGERS
WHERE TRIGNAME = 'EMP_BEFORE_INSERT'
  AND TRIGSCHEMA = CURRENT SCHEMA;
```

### Enabling/Disabling Triggers

```sql
-- Disable a trigger
ALTER TRIGGER EMP_BEFORE_INSERT DISABLE;

-- Enable a trigger
ALTER TRIGGER EMP_BEFORE_INSERT ENABLE;
```

### Dropping Triggers

```sql
-- Drop a specific trigger
DROP TRIGGER ORD_AFTER_INSERT;

-- Drop multiple triggers (must be done individually)
DROP TRIGGER ORDER_VALIDATION_1;
DROP TRIGGER ORDER_VALIDATION_2;
```

## Best Practices

1. **Use appropriate trigger timing**: BEFORE for validation and data modification, AFTER for auditing and logging
2. **Keep triggers simple**: Complex logic should be in stored procedures
3. **Avoid recursive triggers**: Be careful not to create triggers that activate themselves
4. **Use WHEN clause**: Filter unnecessary trigger executions for better performance
5. **Handle errors properly**: Use SIGNAL SQLSTATE for custom error messages
6. **Document triggers**: Include comments explaining the trigger's purpose
7. **Test thoroughly**: Triggers can have unexpected side effects

## Common Use Cases

1. **Audit trails**: Track all changes to sensitive data
2. **Data validation**: Enforce complex business rules
3. **Automatic timestamps**: Set creation/modification dates
4. **Denormalization**: Maintain summary tables
5. **Data archiving**: Move deleted records to archive tables
6. **Notification**: Log events for external processing
7. **Data synchronization**: Keep related tables in sync

## Testing Triggers

```sql
-- Test INSERT trigger
INSERT INTO ORDERS (ORDERID, CUSTOMERID, ITEM, BILL_AMOUNT)
VALUES (123, 'C10', 'LAPTOP', 1500.00);

-- Check order history
SELECT * FROM ORDER_HISTORY WHERE ORDERID = 123;

-- Test UPDATE trigger
UPDATE ORDERS SET ORDER_STATUS = 'SHIPPED' WHERE ORDERID = 123;

-- Verify trigger execution
SELECT * FROM ORDER_HISTORY WHERE ORDERID = 123 ORDER BY DATE_OF_ORDER;
```