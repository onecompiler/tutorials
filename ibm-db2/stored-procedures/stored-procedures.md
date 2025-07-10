# IBM DB2 Stored Procedures

A stored procedure in IBM DB2 is a compiled program that can execute SQL statements and is stored in the database server. DB2 stored procedures are written in SQL PL (SQL Procedural Language) and provide a powerful way to encapsulate business logic, improve performance, and enhance security.

## Syntax

```sql
CREATE PROCEDURE procedure_name (
    IN | OUT | INOUT parameter_name data_type,
    ...
)
LANGUAGE SQL
BEGIN
    -- SQL PL statements
END
```

## Parameter Modes

DB2 supports three parameter modes:

- **IN**: Input parameter (default)
- **OUT**: Output parameter
- **INOUT**: Input/Output parameter

## Basic Examples

### 1. Simple Stored Procedure without Parameters

```sql
CREATE PROCEDURE GET_ALL_CUSTOMERS ()
LANGUAGE SQL
BEGIN
    DECLARE SQLCODE INT DEFAULT 0;
    
    -- Return all customers
    DECLARE c1 CURSOR WITH RETURN FOR
        SELECT * FROM CUSTOMERS;
    
    OPEN c1;
END
```

### 2. Stored Procedure with IN Parameter

```sql
CREATE PROCEDURE GET_CUSTOMER_ORDERS (
    IN p_customer_id VARCHAR(30)
)
LANGUAGE SQL
BEGIN
    DECLARE SQLCODE INT DEFAULT 0;
    
    -- Return orders for specific customer
    DECLARE c1 CURSOR WITH RETURN FOR
        SELECT ORDERID, CUSTOMERID, ITEM, BILLAMOUNT 
        FROM ORDERS 
        WHERE CUSTOMERID = p_customer_id;
    
    OPEN c1;
END
```

### 3. Stored Procedure with OUT Parameter

```sql
CREATE PROCEDURE GET_CUSTOMER_COUNT (
    OUT p_count INTEGER
)
LANGUAGE SQL
BEGIN
    DECLARE SQLCODE INT DEFAULT 0;
    
    SELECT COUNT(*) INTO p_count
    FROM CUSTOMERS;
END
```

### 4. Stored Procedure with Multiple Parameters

```sql
CREATE PROCEDURE GET_ORDER_SUMMARY (
    IN p_customer_id VARCHAR(30),
    OUT p_total_orders INTEGER,
    OUT p_total_amount DECIMAL(10,2)
)
LANGUAGE SQL
BEGIN
    DECLARE SQLCODE INT DEFAULT 0;
    
    -- Get total number of orders
    SELECT COUNT(*) INTO p_total_orders
    FROM ORDERS
    WHERE CUSTOMERID = p_customer_id;
    
    -- Get total amount
    SELECT COALESCE(SUM(BILLAMOUNT), 0) INTO p_total_amount
    FROM ORDERS
    WHERE CUSTOMERID = p_customer_id;
END
```

### 5. Stored Procedure with INOUT Parameter

```sql
CREATE PROCEDURE APPLY_DISCOUNT (
    IN p_customer_id VARCHAR(30),
    IN p_discount_percent DECIMAL(5,2),
    INOUT p_order_amount DECIMAL(10,2)
)
LANGUAGE SQL
BEGIN
    DECLARE SQLCODE INT DEFAULT 0;
    DECLARE v_customer_type VARCHAR(20);
    
    -- Get customer type
    SELECT CUSTOMER_TYPE INTO v_customer_type
    FROM CUSTOMERS
    WHERE CUSTOMERID = p_customer_id;
    
    -- Apply discount based on customer type
    IF v_customer_type = 'PREMIUM' THEN
        SET p_order_amount = p_order_amount * (1 - p_discount_percent / 100);
    END IF;
END
```

## Error Handling with SQLCODE and SQLSTATE

### Using SQLCODE

```sql
CREATE PROCEDURE PROCESS_ORDER (
    IN p_order_id VARCHAR(20),
    OUT p_status VARCHAR(50)
)
LANGUAGE SQL
BEGIN
    DECLARE SQLCODE INT DEFAULT 0;
    DECLARE v_customer_id VARCHAR(30);
    
    -- Declare continue handler for not found
    DECLARE CONTINUE HANDLER FOR NOT FOUND
        SET SQLCODE = 100;
    
    -- Try to get customer ID
    SELECT CUSTOMERID INTO v_customer_id
    FROM ORDERS
    WHERE ORDERID = p_order_id;
    
    IF SQLCODE = 100 THEN
        SET p_status = 'Order not found';
    ELSEIF SQLCODE < 0 THEN
        SET p_status = 'Error occurred: ' || CHAR(SQLCODE);
    ELSE
        SET p_status = 'Order found for customer: ' || v_customer_id;
    END IF;
END
```

### Using SQLSTATE

```sql
CREATE PROCEDURE SAFE_INSERT_ORDER (
    IN p_order_id VARCHAR(20),
    IN p_customer_id VARCHAR(30),
    IN p_item VARCHAR(100),
    IN p_amount DECIMAL(10,2),
    OUT p_result VARCHAR(100)
)
LANGUAGE SQL
BEGIN
    DECLARE v_sqlstate CHAR(5);
    DECLARE v_error_msg VARCHAR(100);
    
    -- Declare exit handler for SQL exceptions
    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        GET DIAGNOSTICS EXCEPTION 1 
            v_sqlstate = RETURNED_SQLSTATE,
            v_error_msg = MESSAGE_TEXT;
        SET p_result = 'Error: ' || v_sqlstate || ' - ' || v_error_msg;
    END;
    
    -- Try to insert order
    INSERT INTO ORDERS (ORDERID, CUSTOMERID, ITEM, BILLAMOUNT)
    VALUES (p_order_id, p_customer_id, p_item, p_amount);
    
    SET p_result = 'Order inserted successfully';
END
```

## Advanced Examples

### 1. Stored Procedure with Cursor Processing

```sql
CREATE PROCEDURE CALCULATE_CUSTOMER_TOTALS ()
LANGUAGE SQL
BEGIN
    DECLARE v_customer_id VARCHAR(30);
    DECLARE v_total DECIMAL(10,2);
    DECLARE v_at_end INT DEFAULT 0;
    DECLARE SQLCODE INT DEFAULT 0;
    
    -- Declare cursor
    DECLARE c1 CURSOR FOR
        SELECT CUSTOMERID, SUM(BILLAMOUNT) as TOTAL
        FROM ORDERS
        GROUP BY CUSTOMERID;
    
    -- Declare continue handler
    DECLARE CONTINUE HANDLER FOR NOT FOUND
        SET v_at_end = 1;
    
    -- Create temporary table for results
    DECLARE GLOBAL TEMPORARY TABLE SESSION.CUSTOMER_TOTALS (
        CUSTOMERID VARCHAR(30),
        TOTAL_AMOUNT DECIMAL(10,2)
    ) ON COMMIT PRESERVE ROWS;
    
    -- Process cursor
    OPEN c1;
    
    FETCH_LOOP: LOOP
        FETCH c1 INTO v_customer_id, v_total;
        
        IF v_at_end = 1 THEN
            LEAVE FETCH_LOOP;
        END IF;
        
        INSERT INTO SESSION.CUSTOMER_TOTALS
        VALUES (v_customer_id, v_total);
    END LOOP;
    
    CLOSE c1;
    
    -- Return results
    DECLARE c2 CURSOR WITH RETURN FOR
        SELECT * FROM SESSION.CUSTOMER_TOTALS
        ORDER BY TOTAL_AMOUNT DESC;
    
    OPEN c2;
END
```

### 2. Stored Procedure with Dynamic SQL

```sql
CREATE PROCEDURE DYNAMIC_QUERY (
    IN p_table_name VARCHAR(128),
    IN p_where_clause VARCHAR(1000)
)
LANGUAGE SQL
BEGIN
    DECLARE v_sql VARCHAR(2000);
    DECLARE v_stmt STATEMENT;
    
    -- Build dynamic SQL
    SET v_sql = 'SELECT * FROM ' || p_table_name;
    
    IF p_where_clause IS NOT NULL AND LENGTH(TRIM(p_where_clause)) > 0 THEN
        SET v_sql = v_sql || ' WHERE ' || p_where_clause;
    END IF;
    
    -- Prepare and execute
    PREPARE v_stmt FROM v_sql;
    
    DECLARE c1 CURSOR WITH RETURN FOR v_stmt;
    OPEN c1;
END
```

### 3. Stored Procedure with Transaction Control

```sql
CREATE PROCEDURE TRANSFER_FUNDS (
    IN p_from_account VARCHAR(20),
    IN p_to_account VARCHAR(20),
    IN p_amount DECIMAL(10,2),
    OUT p_status VARCHAR(100)
)
LANGUAGE SQL
BEGIN
    DECLARE v_balance DECIMAL(10,2);
    DECLARE SQLCODE INT DEFAULT 0;
    
    -- Declare exit handler for exceptions
    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        ROLLBACK;
        SET p_status = 'Transfer failed - transaction rolled back';
    END;
    
    -- Start transaction
    BEGIN ATOMIC
        -- Check balance
        SELECT BALANCE INTO v_balance
        FROM ACCOUNTS
        WHERE ACCOUNT_ID = p_from_account
        FOR UPDATE;
        
        IF v_balance < p_amount THEN
            SIGNAL SQLSTATE '75001'
                SET MESSAGE_TEXT = 'Insufficient funds';
        END IF;
        
        -- Debit from account
        UPDATE ACCOUNTS
        SET BALANCE = BALANCE - p_amount
        WHERE ACCOUNT_ID = p_from_account;
        
        -- Credit to account
        UPDATE ACCOUNTS
        SET BALANCE = BALANCE + p_amount
        WHERE ACCOUNT_ID = p_to_account;
        
        SET p_status = 'Transfer completed successfully';
    END;
END
```

## Working with Different Data Types

### Date and Time Operations

```sql
CREATE PROCEDURE GET_DATE_RANGE_ORDERS (
    IN p_start_date DATE,
    IN p_end_date DATE,
    OUT p_order_count INTEGER
)
LANGUAGE SQL
BEGIN
    DECLARE SQLCODE INT DEFAULT 0;
    
    SELECT COUNT(*) INTO p_order_count
    FROM ORDERS
    WHERE ORDER_DATE BETWEEN p_start_date AND p_end_date;
END
```

### Working with LOBs

```sql
CREATE PROCEDURE STORE_DOCUMENT (
    IN p_doc_id INTEGER,
    IN p_doc_name VARCHAR(200),
    IN p_doc_content CLOB(1M),
    OUT p_result VARCHAR(100)
)
LANGUAGE SQL
BEGIN
    DECLARE SQLCODE INT DEFAULT 0;
    
    INSERT INTO DOCUMENTS (DOC_ID, DOC_NAME, CONTENT, CREATED_DATE)
    VALUES (p_doc_id, p_doc_name, p_doc_content, CURRENT_TIMESTAMP);
    
    SET p_result = 'Document stored successfully';
END
```

## Calling Stored Procedures

### From SQL

```sql
-- Call procedure without parameters
CALL GET_ALL_CUSTOMERS();

-- Call procedure with IN parameter
CALL GET_CUSTOMER_ORDERS('C10');

-- Call procedure with OUT parameter
CALL GET_CUSTOMER_COUNT(?);

-- Call procedure with multiple parameters
CALL GET_ORDER_SUMMARY('C10', ?, ?);
```

### From Application Code

```sql
-- Using parameter markers
CALL GET_ORDER_SUMMARY(?, ?, ?);
```

## Managing Stored Procedures

### View Stored Procedures

```sql
-- List all procedures in current schema
SELECT ROUTINENAME, ROUTINESCHEMA, PARM_COUNT
FROM SYSCAT.ROUTINES
WHERE ROUTINETYPE = 'P'
AND ROUTINESCHEMA = CURRENT_SCHEMA;

-- View procedure definition
SELECT TEXT 
FROM SYSCAT.ROUTINES
WHERE ROUTINENAME = 'GET_CUSTOMER_ORDERS'
AND ROUTINESCHEMA = CURRENT_SCHEMA;
```

### Drop Stored Procedure

```sql
DROP PROCEDURE procedure_name;

-- Example
DROP PROCEDURE GET_CUSTOMER_ORDERS;
```

### Grant Privileges

```sql
-- Grant execute privilege
GRANT EXECUTE ON PROCEDURE GET_CUSTOMER_ORDERS TO USER db2user;

-- Grant to public
GRANT EXECUTE ON PROCEDURE GET_CUSTOMER_ORDERS TO PUBLIC;
```

## Best Practices

1. **Use meaningful parameter names** with prefixes (p_ for parameters, v_ for variables)
2. **Always declare SQLCODE or use exception handlers** for error handling
3. **Use parameter markers (?)** when calling from applications to prevent SQL injection
4. **Document complex logic** with comments
5. **Test thoroughly** with various input scenarios
6. **Consider performance** - use appropriate indexes and avoid unnecessary loops
7. **Use atomic blocks** for transaction consistency
8. **Return result sets using cursors** WITH RETURN clause

## Performance Tips

1. **Compile with optimization**: Use EXPLAIN to analyze execution plans
2. **Minimize network round trips**: Batch operations when possible
3. **Use appropriate isolation levels**: Don't over-isolate unless necessary
4. **Cache frequently used data**: Consider using global temporary tables
5. **Limit result sets**: Use FETCH FIRST n ROWS when appropriate