# Stored Procedures in Firebird

Stored procedures are reusable program units stored in the database that can perform complex operations. Firebird supports powerful stored procedures written in PSQL (Procedural SQL), its built-in procedural language.

## Types of Stored Procedures

Firebird supports two types of stored procedures:

1. **Executable Procedures**: Perform actions, can return a single row of values
2. **Selectable Procedures**: Return multiple rows, used with SELECT statement

## Basic Syntax

### Executable Stored Procedure

```sql
CREATE [OR ALTER] PROCEDURE procedure_name
    [(parameter1 datatype, parameter2 datatype, ...)]
    [RETURNS (output1 datatype, output2 datatype, ...)]
AS
[DECLARE VARIABLE variable_name datatype;]
BEGIN
    /* Procedure body */
    [SUSPEND;]  -- For selectable procedures
END
```

## Creating Simple Procedures

### 1. Basic Procedure (No Parameters)

```sql
CREATE PROCEDURE HELLO_WORLD
AS
BEGIN
    /* This procedure does nothing but can be called */
    EXIT;
END
```

Execute:
```sql
EXECUTE PROCEDURE HELLO_WORLD;
```

### 2. Procedure with Input Parameters

```sql
CREATE PROCEDURE ADD_EMPLOYEE (
    EMP_NAME VARCHAR(50),
    DEPT_ID INTEGER,
    SALARY NUMERIC(10,2)
)
AS
BEGIN
    INSERT INTO EMPLOYEES (EMP_NAME, DEPT_ID, SALARY, HIRE_DATE)
    VALUES (:EMP_NAME, :DEPT_ID, :SALARY, CURRENT_DATE);
END
```

Execute:
```sql
EXECUTE PROCEDURE ADD_EMPLOYEE('John Doe', 2, 55000);
```

### 3. Procedure with Output Parameters

```sql
CREATE PROCEDURE GET_EMPLOYEE_COUNT
RETURNS (TOTAL_COUNT INTEGER)
AS
BEGIN
    SELECT COUNT(*) FROM EMPLOYEES
    INTO :TOTAL_COUNT;
END
```

Execute:
```sql
EXECUTE PROCEDURE GET_EMPLOYEE_COUNT;
```

## Selectable Stored Procedures

Procedures that return multiple rows using SUSPEND:

```sql
CREATE PROCEDURE GET_EMPLOYEES_BY_DEPT (
    DEPT_ID INTEGER
)
RETURNS (
    EMP_ID INTEGER,
    EMP_NAME VARCHAR(50),
    SALARY NUMERIC(10,2)
)
AS
BEGIN
    FOR SELECT EMP_ID, EMP_NAME, SALARY
        FROM EMPLOYEES
        WHERE DEPT_ID = :DEPT_ID
        INTO :EMP_ID, :EMP_NAME, :SALARY
    DO
        SUSPEND;
END
```

Use with SELECT:
```sql
SELECT * FROM GET_EMPLOYEES_BY_DEPT(2);
```

## Variables and Control Structures

### Declaring Variables

```sql
CREATE PROCEDURE CALCULATE_BONUS (
    EMP_ID INTEGER,
    BONUS_PERCENT NUMERIC(5,2)
)
RETURNS (BONUS_AMOUNT NUMERIC(10,2))
AS
DECLARE VARIABLE SALARY NUMERIC(10,2);
DECLARE VARIABLE BASE_BONUS NUMERIC(10,2);
BEGIN
    -- Get employee salary
    SELECT SALARY FROM EMPLOYEES
    WHERE EMP_ID = :EMP_ID
    INTO :SALARY;
    
    -- Calculate bonus
    BASE_BONUS = SALARY * BONUS_PERCENT / 100;
    
    -- Apply minimum bonus rule
    IF (BASE_BONUS < 1000) THEN
        BONUS_AMOUNT = 1000;
    ELSE
        BONUS_AMOUNT = BASE_BONUS;
END
```

### IF-THEN-ELSE Statement

```sql
CREATE PROCEDURE CHECK_INVENTORY (
    PRODUCT_ID INTEGER,
    REQUESTED_QTY INTEGER
)
RETURNS (
    STATUS VARCHAR(20),
    AVAILABLE_QTY INTEGER
)
AS
DECLARE VARIABLE CURRENT_QTY INTEGER;
BEGIN
    SELECT QUANTITY FROM INVENTORY
    WHERE PRODUCT_ID = :PRODUCT_ID
    INTO :CURRENT_QTY;
    
    AVAILABLE_QTY = CURRENT_QTY;
    
    IF (CURRENT_QTY IS NULL) THEN
        STATUS = 'NOT_FOUND';
    ELSE IF (CURRENT_QTY >= REQUESTED_QTY) THEN
        STATUS = 'AVAILABLE';
    ELSE IF (CURRENT_QTY > 0) THEN
        STATUS = 'PARTIAL';
    ELSE
        STATUS = 'OUT_OF_STOCK';
END
```

### WHILE Loop

```sql
CREATE PROCEDURE GENERATE_SERIES (
    START_NUM INTEGER,
    END_NUM INTEGER
)
RETURNS (NUM INTEGER)
AS
BEGIN
    NUM = START_NUM;
    WHILE (NUM <= END_NUM) DO
    BEGIN
        SUSPEND;
        NUM = NUM + 1;
    END
END
```

Usage:
```sql
SELECT * FROM GENERATE_SERIES(1, 10);
```

### FOR SELECT Loop

```sql
CREATE PROCEDURE CALCULATE_DEPT_TOTALS
RETURNS (
    DEPT_ID INTEGER,
    DEPT_NAME VARCHAR(50),
    TOTAL_SALARY NUMERIC(15,2),
    EMP_COUNT INTEGER
)
AS
BEGIN
    FOR SELECT 
        D.DEPT_ID,
        D.DEPT_NAME,
        SUM(E.SALARY),
        COUNT(E.EMP_ID)
    FROM DEPARTMENTS D
    LEFT JOIN EMPLOYEES E ON D.DEPT_ID = E.DEPT_ID
    GROUP BY D.DEPT_ID, D.DEPT_NAME
    INTO :DEPT_ID, :DEPT_NAME, :TOTAL_SALARY, :EMP_COUNT
    DO
        SUSPEND;
END
```

## Exception Handling

```sql
CREATE PROCEDURE SAFE_DIVIDE (
    NUMERATOR DOUBLE PRECISION,
    DENOMINATOR DOUBLE PRECISION
)
RETURNS (RESULT DOUBLE PRECISION)
AS
BEGIN
    IF (DENOMINATOR = 0) THEN
    BEGIN
        RESULT = NULL;
        EXCEPTION DIVISION_BY_ZERO 'Cannot divide by zero';
    END
    ELSE
        RESULT = NUMERATOR / DENOMINATOR;
    
    WHEN ANY DO
    BEGIN
        RESULT = NULL;
        EXCEPTION;  -- Re-raise the exception
    END
END
```

## Cursors in Procedures

```sql
CREATE PROCEDURE PROCESS_ORDERS
AS
DECLARE VARIABLE ORDER_ID INTEGER;
DECLARE VARIABLE CUSTOMER_ID INTEGER;
DECLARE VARIABLE TOTAL NUMERIC(10,2);
DECLARE C_ORDERS CURSOR FOR (
    SELECT ORDER_ID, CUSTOMER_ID, TOTAL_AMOUNT
    FROM ORDERS
    WHERE STATUS = 'PENDING'
);
BEGIN
    OPEN C_ORDERS;
    WHILE (1=1) DO
    BEGIN
        FETCH C_ORDERS INTO :ORDER_ID, :CUSTOMER_ID, :TOTAL;
        IF (ROW_COUNT = 0) THEN LEAVE;
        
        -- Process each order
        UPDATE ORDERS SET STATUS = 'PROCESSED'
        WHERE ORDER_ID = :ORDER_ID;
        
        -- Log the processing
        INSERT INTO ORDER_LOG (ORDER_ID, PROCESS_DATE)
        VALUES (:ORDER_ID, CURRENT_TIMESTAMP);
    END
    CLOSE C_ORDERS;
END
```

## Recursive Procedures

```sql
CREATE PROCEDURE FACTORIAL (N INTEGER)
RETURNS (RESULT BIGINT)
AS
DECLARE VARIABLE TEMP BIGINT;
BEGIN
    IF (N <= 1) THEN
        RESULT = 1;
    ELSE
    BEGIN
        EXECUTE PROCEDURE FACTORIAL(:N - 1) RETURNING_VALUES :TEMP;
        RESULT = N * TEMP;
    END
END
```

## Procedures with Transactions

```sql
CREATE PROCEDURE TRANSFER_FUNDS (
    FROM_ACCOUNT INTEGER,
    TO_ACCOUNT INTEGER,
    AMOUNT NUMERIC(10,2)
)
AS
DECLARE VARIABLE BALANCE NUMERIC(10,2);
BEGIN
    -- Start autonomous transaction
    IN AUTONOMOUS TRANSACTION DO
    BEGIN
        -- Check source account balance
        SELECT BALANCE FROM ACCOUNTS
        WHERE ACCOUNT_ID = :FROM_ACCOUNT
        INTO :BALANCE;
        
        IF (BALANCE < AMOUNT) THEN
            EXCEPTION INSUFFICIENT_FUNDS 'Insufficient balance';
        
        -- Debit source account
        UPDATE ACCOUNTS SET BALANCE = BALANCE - :AMOUNT
        WHERE ACCOUNT_ID = :FROM_ACCOUNT;
        
        -- Credit destination account
        UPDATE ACCOUNTS SET BALANCE = BALANCE + :AMOUNT
        WHERE ACCOUNT_ID = :TO_ACCOUNT;
        
        -- Log transaction
        INSERT INTO TRANSACTION_LOG (FROM_ACC, TO_ACC, AMOUNT, TRANS_DATE)
        VALUES (:FROM_ACCOUNT, :TO_ACCOUNT, :AMOUNT, CURRENT_TIMESTAMP);
    END
END
```

## Managing Stored Procedures

### View Existing Procedures

```sql
-- List all procedures
SELECT RDB$PROCEDURE_NAME 
FROM RDB$PROCEDURES
WHERE RDB$SYSTEM_FLAG = 0
ORDER BY RDB$PROCEDURE_NAME;

-- View procedure source
SELECT RDB$PROCEDURE_SOURCE
FROM RDB$PROCEDURES
WHERE RDB$PROCEDURE_NAME = 'GET_EMPLOYEES_BY_DEPT';
```

### Alter Procedure

```sql
ALTER PROCEDURE GET_EMPLOYEE_COUNT
RETURNS (TOTAL_COUNT INTEGER, ACTIVE_COUNT INTEGER)
AS
BEGIN
    SELECT COUNT(*) FROM EMPLOYEES
    INTO :TOTAL_COUNT;
    
    SELECT COUNT(*) FROM EMPLOYEES
    WHERE STATUS = 'ACTIVE'
    INTO :ACTIVE_COUNT;
END
```

### Drop Procedure

```sql
DROP PROCEDURE procedure_name;
```

### Grant Permissions

```sql
-- Grant execute permission
GRANT EXECUTE ON PROCEDURE GET_EMPLOYEES_BY_DEPT TO USER john_doe;
GRANT EXECUTE ON PROCEDURE ADD_EMPLOYEE TO ROLE managers;

-- Revoke permission
REVOKE EXECUTE ON PROCEDURE ADD_EMPLOYEE FROM USER john_doe;
```

## Best Practices

1. **Use meaningful names**: Prefix with sp_ or use descriptive names
2. **Parameter naming**: Use prefixes (I_ for input, O_ for output)
3. **Error handling**: Always include exception handling
4. **Documentation**: Comment complex logic
5. **Avoid long procedures**: Break into smaller, reusable procedures
6. **Use transactions wisely**: Consider autonomous transactions
7. **Test thoroughly**: Include edge cases

## Common Patterns

### 1. Logging Pattern

```sql
CREATE PROCEDURE LOG_ACTIVITY (
    USER_ID INTEGER,
    ACTION VARCHAR(50)
)
AS
BEGIN
    INSERT INTO ACTIVITY_LOG (USER_ID, ACTION, LOG_TIME)
    VALUES (:USER_ID, :ACTION, CURRENT_TIMESTAMP);
    
    WHEN ANY DO
    BEGIN
        -- Silently fail, don't break main operation
        EXIT;
    END
END
```

### 2. Data Validation Pattern

```sql
CREATE PROCEDURE VALIDATE_EMAIL (
    EMAIL VARCHAR(255)
)
RETURNS (IS_VALID BOOLEAN)
AS
BEGIN
    IS_VALID = FALSE;
    
    IF (EMAIL CONTAINING '@' AND EMAIL CONTAINING '.') THEN
        IS_VALID = TRUE;
END
```

### 3. Batch Processing Pattern

```sql
CREATE PROCEDURE PROCESS_BATCH (
    BATCH_SIZE INTEGER = 100
)
AS
DECLARE VARIABLE PROCESSED INTEGER = 0;
BEGIN
    WHILE (PROCESSED < BATCH_SIZE) DO
    BEGIN
        -- Process one record
        UPDATE QUEUE SET STATUS = 'PROCESSED'
        WHERE ID = (
            SELECT FIRST 1 ID FROM QUEUE
            WHERE STATUS = 'PENDING'
        );
        
        IF (ROW_COUNT = 0) THEN LEAVE;
        
        PROCESSED = PROCESSED + 1;
    END
END
```

## Next Steps

- Learn about triggers and their interaction with procedures
- Explore user-defined functions (UDFs)
- Study performance optimization for procedures
- Practice with complex business logic implementation