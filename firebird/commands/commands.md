
# Firebird Commands

This guide covers essential SQL commands and ISQL utility commands for working with Firebird databases.

## DDL Commands (Data Definition Language)

### Database Commands

```sql
-- Create database
CREATE DATABASE 'path/to/database.fdb'
    USER 'username' PASSWORD 'password'
    PAGE_SIZE 16384
    DEFAULT CHARACTER SET UTF8;

-- Connect to database
CONNECT 'path/to/database.fdb' 
    USER 'username' PASSWORD 'password';

-- Drop database (be careful!)
DROP DATABASE;

-- Alter database
ALTER DATABASE ADD FILE 'secondary.fdb';
ALTER DATABASE SET DEFAULT CHARACTER SET UTF8;
```

### Table Commands

```sql
-- Create table
CREATE TABLE employees (
    emp_id INTEGER NOT NULL PRIMARY KEY,
    emp_name VARCHAR(50) NOT NULL,
    dept_id INTEGER,
    salary NUMERIC(10,2),
    hire_date DATE DEFAULT CURRENT_DATE
);

-- Alter table
ALTER TABLE employees ADD email VARCHAR(100);
ALTER TABLE employees ALTER COLUMN salary TYPE NUMERIC(12,2);
ALTER TABLE employees DROP CONSTRAINT pk_employees;

-- Drop table
DROP TABLE employees;

-- Recreate table
RECREATE TABLE employees (...);  -- Drops if exists, then creates
```

### Index Commands

```sql
-- Create index
CREATE INDEX idx_emp_name ON employees(emp_name);
CREATE UNIQUE INDEX idx_emp_email ON employees(email);
CREATE DESCENDING INDEX idx_salary_desc ON employees(salary);

-- Drop index
DROP INDEX idx_emp_name;

-- Recompute index statistics
SET STATISTICS INDEX idx_emp_name;
```

## DML Commands (Data Manipulation Language)

### INSERT

```sql
-- Basic insert
INSERT INTO employees (emp_id, emp_name, dept_id, salary)
VALUES (1, 'John Doe', 101, 50000);

-- Insert with returning
INSERT INTO employees (emp_name, dept_id)
VALUES ('Jane Smith', 102)
RETURNING emp_id;

-- Insert from select
INSERT INTO employees_archive
SELECT * FROM employees WHERE hire_date < '2020-01-01';
```

### UPDATE

```sql
-- Basic update
UPDATE employees 
SET salary = salary * 1.1 
WHERE dept_id = 101;

-- Update with returning
UPDATE employees 
SET salary = 60000 
WHERE emp_id = 1
RETURNING OLD.salary, NEW.salary;

-- Update or insert (MERGE)
UPDATE OR INSERT INTO employees (emp_id, emp_name, salary)
VALUES (1, 'John Doe', 55000)
MATCHING (emp_id);
```

### DELETE

```sql
-- Basic delete
DELETE FROM employees WHERE emp_id = 1;

-- Delete with returning
DELETE FROM employees 
WHERE hire_date < '2019-01-01'
RETURNING emp_id, emp_name;

-- Delete all records (use with caution)
DELETE FROM employees;
```

### SELECT

```sql
-- Basic select
SELECT * FROM employees;
SELECT emp_name, salary FROM employees WHERE dept_id = 101;

-- With joins
SELECT e.emp_name, d.dept_name
FROM employees e
INNER JOIN departments d ON e.dept_id = d.dept_id;

-- With aggregates
SELECT dept_id, COUNT(*), AVG(salary)
FROM employees
GROUP BY dept_id
HAVING COUNT(*) > 5;

-- Common Table Expressions (CTE)
WITH high_earners AS (
    SELECT * FROM employees WHERE salary > 70000
)
SELECT * FROM high_earners;
```

### MERGE

```sql
MERGE INTO target_table t
USING source_table s ON t.id = s.id
WHEN MATCHED THEN 
    UPDATE SET t.value = s.value
WHEN NOT MATCHED THEN 
    INSERT (id, value) VALUES (s.id, s.value);
```

## DCL Commands (Data Control Language)

### User Management

```sql
-- Create user
CREATE USER john_doe PASSWORD 'secure_password';

-- Alter user
ALTER USER john_doe SET PASSWORD 'new_password';
ALTER USER john_doe USING PLUGIN Srp;

-- Drop user
DROP USER john_doe;
```

### Privileges

```sql
-- Grant privileges
GRANT SELECT, INSERT, UPDATE ON employees TO john_doe;
GRANT ALL ON DATABASE TO john_doe;
GRANT EXECUTE ON PROCEDURE get_employee_count TO PUBLIC;

-- Revoke privileges
REVOKE UPDATE ON employees FROM john_doe;
REVOKE ALL ON DATABASE FROM john_doe;
```

### Roles

```sql
-- Create role
CREATE ROLE managers;

-- Grant privileges to role
GRANT SELECT, UPDATE ON employees TO managers;

-- Grant role to user
GRANT managers TO john_doe;

-- Drop role
DROP ROLE managers;
```

## Transaction Commands

```sql
-- Start transaction
SET TRANSACTION READ WRITE WAIT ISOLATION LEVEL SNAPSHOT;

-- Commit transaction
COMMIT;

-- Rollback transaction
ROLLBACK;

-- Savepoints
SAVEPOINT sp1;
ROLLBACK TO sp1;
RELEASE SAVEPOINT sp1;

-- Retain context after commit
COMMIT RETAIN;
```

## ISQL Interactive Commands

| Command | Description |
|---------|-------------|
| `CONNECT 'database.fdb'` | Connect to database |
| `CREATE DATABASE 'new.fdb'` | Create new database |
| `SHOW TABLES` | List all tables |
| `SHOW TABLE tablename` | Show table structure |
| `SHOW PROCEDURES` | List all stored procedures |
| `SHOW TRIGGERS` | List all triggers |
| `SHOW DOMAINS` | List all domains |
| `SHOW GENERATORS` | List all generators/sequences |
| `SHOW SYSTEM` | Show system tables |
| `SHOW DATABASE` | Show database information |
| `SHOW VERSION` | Show Firebird version |

### ISQL Settings

| Command | Description |
|---------|-------------|
| `SET SQL DIALECT 3` | Set SQL dialect (1 or 3) |
| `SET AUTODDL ON/OFF` | Auto-commit DDL statements |
| `SET ECHO ON/OFF` | Echo commands |
| `SET COUNT ON/OFF` | Show row count |
| `SET STATISTICS ON/OFF` | Show query statistics |
| `SET PLAN ON/OFF` | Show query execution plan |
| `SET TIME ON/OFF` | Show time for each statement |
| `SET TERMINATOR ;` | Change statement terminator |
| `SET HEADING ON/OFF` | Show/hide column headings |
| `SET WIDTH column n` | Set display width for column |

### ISQL Utility Commands

| Command | Description |
|---------|-------------|
| `INPUT filename.sql` | Execute SQL script file |
| `OUTPUT filename.txt` | Redirect output to file |
| `OUTPUT` | Return output to console |
| `SHELL command` | Execute OS command |
| `EDIT [filename]` | Edit file or last command |
| `HELP` | Show help |
| `HELP SET` | Show SET options |
| `EXIT` | Exit and commit |
| `QUIT` | Exit and rollback |

## System Functions

### String Functions

```sql
SELECT UPPER('hello');           -- HELLO
SELECT LOWER('HELLO');           -- hello
SELECT TRIM('  hello  ');        -- hello
SELECT SUBSTRING('Firebird' FROM 1 FOR 4);  -- Fire
SELECT CHAR_LENGTH('Firebird');  -- 8
SELECT POSITION('bird' IN 'Firebird');  -- 5
SELECT REPLACE('Hello', 'l', 'L');  -- HeLLo
```

### Date/Time Functions

```sql
SELECT CURRENT_DATE;             -- Current date
SELECT CURRENT_TIME;             -- Current time
SELECT CURRENT_TIMESTAMP;        -- Current timestamp
SELECT EXTRACT(YEAR FROM CURRENT_DATE);  -- Extract year
SELECT DATEADD(DAY, 7, CURRENT_DATE);    -- Add 7 days
SELECT DATEDIFF(DAY, '2024-01-01', CURRENT_DATE);  -- Days difference
```

### Numeric Functions

```sql
SELECT ABS(-10);                 -- 10
SELECT ROUND(3.14159, 2);        -- 3.14
SELECT FLOOR(3.7);               -- 3
SELECT CEILING(3.1);             -- 4
SELECT POWER(2, 3);              -- 8
SELECT SQRT(16);                 -- 4
SELECT MOD(10, 3);               -- 1
```

### Aggregate Functions

```sql
SELECT COUNT(*) FROM employees;
SELECT SUM(salary) FROM employees;
SELECT AVG(salary) FROM employees;
SELECT MIN(salary) FROM employees;
SELECT MAX(salary) FROM employees;
SELECT LIST(emp_name) FROM employees;  -- Concatenate values
```

## Best Practices

1. **Use transactions**: Always work within transactions for data consistency
2. **Use prepared statements**: For repeated queries with parameters
3. **Create indexes**: On columns used in WHERE, JOIN, and ORDER BY
4. **Use appropriate data types**: Don't use VARCHAR for numbers
5. **Regular maintenance**: Run database statistics and sweep regularly

## Next Steps

- Learn about stored procedures and triggers
- Explore Firebird-specific features
- Understand transaction isolation levels
- Master query optimization techniques
