# Firebird-Specific Features

This guide covers unique and advanced features that set Firebird apart from other database systems. Understanding these features will help you leverage Firebird's full potential.

## Generators and Sequences

### Generators (Legacy)

Generators are Firebird's traditional way of creating auto-incrementing values:

```sql
-- Create a generator
CREATE GENERATOR gen_customer_id;

-- Set initial value
SET GENERATOR gen_customer_id TO 1000;

-- Get next value
SELECT GEN_ID(gen_customer_id, 1) FROM RDB$DATABASE;

-- Use in trigger
SET TERM ^ ;
CREATE TRIGGER customers_bi FOR customers
ACTIVE BEFORE INSERT POSITION 0
AS
BEGIN
    IF (NEW.customer_id IS NULL) THEN
        NEW.customer_id = GEN_ID(gen_customer_id, 1);
END^
SET TERM ; ^
```

### Sequences (Firebird 3.0+)

Modern SQL-standard sequences:

```sql
-- Create sequence
CREATE SEQUENCE seq_order_id START WITH 1000;

-- Get next value
SELECT NEXT VALUE FOR seq_order_id FROM RDB$DATABASE;

-- Use in INSERT
INSERT INTO orders (order_id, customer_id)
VALUES (NEXT VALUE FOR seq_order_id, 100);

-- Alter sequence
ALTER SEQUENCE seq_order_id RESTART WITH 5000;

-- Drop sequence
DROP SEQUENCE seq_order_id;
```

## Domains

Domains are reusable column definitions with constraints:

```sql
-- Create domains
CREATE DOMAIN d_email AS VARCHAR(255)
    CHECK (VALUE LIKE '%@%.%' OR VALUE IS NULL);

CREATE DOMAIN d_positive_money AS NUMERIC(15,2)
    DEFAULT 0
    CHECK (VALUE >= 0);

CREATE DOMAIN d_phone AS VARCHAR(20)
    CHECK (VALUE SIMILAR TO '[0-9\-\+\(\) ]+' OR VALUE IS NULL);

CREATE DOMAIN d_percentage AS NUMERIC(5,2)
    CHECK (VALUE BETWEEN 0 AND 100);

-- Use domains in table
CREATE TABLE customers (
    customer_id INTEGER NOT NULL PRIMARY KEY,
    email d_email,
    phone d_phone,
    credit_limit d_positive_money,
    discount_rate d_percentage DEFAULT 0
);

-- Alter domain
ALTER DOMAIN d_email ADD CONSTRAINT CHECK (CHAR_LENGTH(VALUE) <= 100);

-- Drop domain
DROP DOMAIN d_phone;
```

## Computed Columns

Columns with values calculated from other columns:

```sql
CREATE TABLE order_items (
    order_id INTEGER NOT NULL,
    product_id INTEGER NOT NULL,
    quantity INTEGER NOT NULL,
    unit_price NUMERIC(10,2) NOT NULL,
    line_total COMPUTED BY (quantity * unit_price),
    tax_amount COMPUTED BY (quantity * unit_price * 0.1),
    total_with_tax COMPUTED BY (quantity * unit_price * 1.1),
    PRIMARY KEY (order_id, product_id)
);

-- Computed columns in queries
SELECT 
    product_id,
    quantity,
    unit_price,
    line_total,  -- Automatically calculated
    total_with_tax
FROM order_items
WHERE line_total > 100;
```

## Arrays

Firebird supports multi-dimensional arrays:

```sql
-- Create table with arrays
CREATE TABLE matrix_data (
    matrix_id INTEGER PRIMARY KEY,
    vector INTEGER[5],  -- 1D array with 5 elements
    matrix DOUBLE PRECISION[3,3],  -- 2D array 3x3
    temperatures FLOAT[1:7],  -- Array with specific bounds
    sales_data NUMERIC(10,2)[1:12, 1:31]  -- Month x Day
);

-- Insert array data
INSERT INTO matrix_data (matrix_id, vector, temperatures)
VALUES (
    1, 
    [1, 2, 3, 4, 5],
    [20.5, 21.0, 19.8, 22.1, 23.0, 21.5, 20.0]
);

-- Access array elements
SELECT 
    vector[1] AS first_element,
    vector[3] AS third_element,
    temperatures[5] AS thursday_temp
FROM matrix_data
WHERE matrix_id = 1;

-- Array slicing
SELECT vector[1:3] FROM matrix_data;  -- Elements 1 to 3
```

## BLOB Operations

Binary Large Objects for storing large data:

```sql
-- Create table with BLOBs
CREATE TABLE documents (
    doc_id INTEGER PRIMARY KEY,
    doc_name VARCHAR(255),
    content BLOB SUB_TYPE 1,  -- Text BLOB
    pdf_file BLOB SUB_TYPE 0,  -- Binary BLOB
    metadata BLOB SUB_TYPE 1 SEGMENT SIZE 1024
);

-- BLOB filters for conversion
DECLARE EXTERNAL FUNCTION string2blob
    CSTRING(32767)
    RETURNS BLOB
    ENTRY_POINT 'string2blob'
    MODULE_NAME 'fbudf';

-- Working with text BLOBs
INSERT INTO documents (doc_id, doc_name, content)
VALUES (1, 'README', 'This is a long text document...');

-- Substring on BLOB
SELECT 
    doc_name,
    SUBSTRING(content FROM 1 FOR 100) AS preview
FROM documents;
```

## Events

Database events for inter-process communication:

```sql
-- Post an event
EXECUTE BLOCK
AS
BEGIN
    POST_EVENT 'DATA_UPDATED';
END;

-- Post event from trigger
CREATE TRIGGER orders_ai FOR orders
ACTIVE AFTER INSERT
AS
BEGIN
    POST_EVENT 'NEW_ORDER';
    
    -- Conditional events
    IF (NEW.total_amount > 10000) THEN
        POST_EVENT 'HIGH_VALUE_ORDER';
END;

-- Post event with stored procedure
CREATE PROCEDURE notify_low_stock
AS
DECLARE VARIABLE product_name VARCHAR(100);
BEGIN
    FOR SELECT product_name 
        FROM products 
        WHERE stock_quantity < 10
        INTO :product_name
    DO
    BEGIN
        POST_EVENT 'LOW_STOCK_' || :product_name;
    END
END;
```

## Window Functions

Analytical functions for complex calculations:

```sql
-- Row numbering
SELECT 
    emp_name,
    dept_id,
    salary,
    ROW_NUMBER() OVER (ORDER BY salary DESC) AS salary_rank,
    ROW_NUMBER() OVER (PARTITION BY dept_id ORDER BY salary DESC) AS dept_rank
FROM employees;

-- Running totals and averages
SELECT 
    order_date,
    total_amount,
    SUM(total_amount) OVER (ORDER BY order_date) AS running_total,
    AVG(total_amount) OVER (
        ORDER BY order_date 
        ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
    ) AS moving_avg_7days
FROM orders;

-- Ranking functions
SELECT 
    product_name,
    category,
    price,
    RANK() OVER (PARTITION BY category ORDER BY price DESC) AS price_rank,
    DENSE_RANK() OVER (PARTITION BY category ORDER BY price DESC) AS dense_rank,
    PERCENT_RANK() OVER (PARTITION BY category ORDER BY price) AS percent_rank
FROM products;

-- Lead and Lag
SELECT 
    order_date,
    total_amount,
    LAG(total_amount, 1) OVER (ORDER BY order_date) AS prev_amount,
    LEAD(total_amount, 1) OVER (ORDER BY order_date) AS next_amount,
    total_amount - LAG(total_amount, 1) OVER (ORDER BY order_date) AS change
FROM orders;
```

## Common Table Expressions (Recursive)

Recursive CTEs for hierarchical data:

```sql
-- Employee hierarchy
WITH RECURSIVE emp_hierarchy AS (
    -- Anchor: CEO (no manager)
    SELECT 
        emp_id,
        emp_name,
        manager_id,
        0 AS level,
        CAST(emp_name AS VARCHAR(1000)) AS path
    FROM employees
    WHERE manager_id IS NULL
    
    UNION ALL
    
    -- Recursive: all other employees
    SELECT 
        e.emp_id,
        e.emp_name,
        e.manager_id,
        h.level + 1,
        h.path || ' > ' || e.emp_name
    FROM employees e
    INNER JOIN emp_hierarchy h ON e.manager_id = h.emp_id
)
SELECT * FROM emp_hierarchy
ORDER BY path;

-- Calculate factorial using recursive CTE
WITH RECURSIVE factorial AS (
    SELECT 
        1 AS n,
        CAST(1 AS BIGINT) AS fact
    
    UNION ALL
    
    SELECT 
        n + 1,
        fact * (n + 1)
    FROM factorial
    WHERE n < 10
)
SELECT n, fact FROM factorial;
```

## External Functions (UDFs)

User-Defined Functions extend Firebird's capabilities:

```sql
-- Declare external function
DECLARE EXTERNAL FUNCTION add_days
    DATE,
    INTEGER
RETURNS DATE
ENTRY_POINT 'addDay'
MODULE_NAME 'fbudf';

-- Use external function
SELECT 
    order_date,
    add_days(order_date, 30) AS due_date
FROM orders;

-- Create internal UDF (Firebird 3.0+)
CREATE FUNCTION fn_calculate_tax (amount NUMERIC(15,2), rate NUMERIC(5,2))
RETURNS NUMERIC(15,2)
AS
BEGIN
    RETURN amount * rate / 100;
END;

-- Use internal function
SELECT 
    order_id,
    total_amount,
    fn_calculate_tax(total_amount, 8.5) AS tax_amount
FROM orders;
```

## MERGE Statement

Upsert operations:

```sql
MERGE INTO products_inventory AS target
USING (
    SELECT 
        product_id,
        SUM(quantity) AS total_quantity
    FROM incoming_shipments
    GROUP BY product_id
) AS source
ON target.product_id = source.product_id
WHEN MATCHED THEN
    UPDATE SET 
        target.quantity = target.quantity + source.total_quantity,
        target.last_updated = CURRENT_TIMESTAMP
WHEN NOT MATCHED THEN
    INSERT (product_id, quantity, last_updated)
    VALUES (source.product_id, source.total_quantity, CURRENT_TIMESTAMP);
```

## Global Temporary Tables

Session-specific or transaction-specific temporary storage:

```sql
-- Create global temporary table (data persists for connection)
CREATE GLOBAL TEMPORARY TABLE temp_calculations (
    calc_id INTEGER,
    result NUMERIC(15,2),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
) ON COMMIT PRESERVE ROWS;

-- Create transaction-specific temporary table
CREATE GLOBAL TEMPORARY TABLE temp_batch_data (
    batch_id INTEGER,
    data_value VARCHAR(1000)
) ON COMMIT DELETE ROWS;

-- Use temporary tables
INSERT INTO temp_calculations (calc_id, result)
SELECT 
    product_id,
    SUM(quantity * unit_price)
FROM order_items
GROUP BY product_id;
```

## Monitoring Tables

System monitoring capabilities:

```sql
-- Current connections
SELECT 
    mon$attachment_id,
    mon$user,
    mon$remote_address,
    mon$timestamp,
    mon$state
FROM mon$attachments
WHERE mon$attachment_id <> CURRENT_CONNECTION;

-- Active statements
SELECT 
    a.mon$user,
    s.mon$sql_text,
    s.mon$timestamp,
    s.mon$state
FROM mon$statements s
JOIN mon$attachments a ON s.mon$attachment_id = a.mon$attachment_id
WHERE s.mon$state = 1;  -- Active

-- Database statistics
SELECT 
    mon$page_reads,
    mon$page_writes,
    mon$page_fetches,
    mon$memory_allocated,
    mon$memory_used
FROM mon$database;

-- Table access statistics
SELECT 
    mon$table_name,
    mon$record_seq_reads,
    mon$record_idx_reads,
    mon$record_inserts,
    mon$record_updates,
    mon$record_deletes
FROM mon$record_stats
WHERE mon$table_name NOT STARTING WITH 'RDB$'
ORDER BY mon$record_seq_reads DESC;
```

## Package Support (Firebird 3.0+)

Organize related procedures and functions:

```sql
-- Create package header
CREATE PACKAGE employee_pkg
AS
BEGIN
    PROCEDURE hire_employee (
        first_name VARCHAR(50),
        last_name VARCHAR(50),
        dept_id INTEGER
    ) RETURNS (new_emp_id INTEGER);
    
    FUNCTION get_employee_count (dept_id INTEGER)
    RETURNS INTEGER;
    
    PROCEDURE terminate_employee (emp_id INTEGER);
END;

-- Create package body
CREATE PACKAGE BODY employee_pkg
AS
BEGIN
    PROCEDURE hire_employee (
        first_name VARCHAR(50),
        last_name VARCHAR(50),
        dept_id INTEGER
    ) RETURNS (new_emp_id INTEGER)
    AS
    BEGIN
        new_emp_id = NEXT VALUE FOR seq_employee_id;
        INSERT INTO employees (emp_id, first_name, last_name, dept_id)
        VALUES (:new_emp_id, :first_name, :last_name, :dept_id);
    END
    
    FUNCTION get_employee_count (dept_id INTEGER)
    RETURNS INTEGER
    AS
    DECLARE VARIABLE emp_count INTEGER;
    BEGIN
        SELECT COUNT(*) FROM employees
        WHERE dept_id = :dept_id
        INTO :emp_count;
        RETURN emp_count;
    END
    
    PROCEDURE terminate_employee (emp_id INTEGER)
    AS
    BEGIN
        UPDATE employees 
        SET status = 'TERMINATED',
            termination_date = CURRENT_DATE
        WHERE emp_id = :emp_id;
    END
END;

-- Use package
EXECUTE PROCEDURE employee_pkg.hire_employee('John', 'Doe', 10);
SELECT employee_pkg.get_employee_count(10) FROM RDB$DATABASE;
```

## Best Practices

1. **Use domains** for consistent data types across tables
2. **Leverage computed columns** for derived values
3. **Implement events** for real-time notifications
4. **Use window functions** for complex analytics
5. **Monitor performance** with MON$ tables
6. **Organize code** with packages
7. **Use temporary tables** for complex processing

## Next Steps

- Explore database encryption features
- Learn about replication capabilities
- Study performance optimization techniques
- Understand backup and recovery strategies
- Master advanced security features