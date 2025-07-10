
# Constraints in Firebird

Constraints are rules that define what values can be inserted or updated in database tables. They play a crucial role in maintaining data integrity and enforcing business rules at the database level.

## Types of Constraints

### 1. NOT NULL Constraint

Ensures that a column cannot contain NULL values.

```sql
CREATE TABLE employees (
    emp_id INTEGER NOT NULL,
    emp_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL,
    phone VARCHAR(20)  -- This can be NULL
);

-- This will fail
INSERT INTO employees (emp_id, emp_name) 
VALUES (1, NULL);  -- Error: emp_name cannot be NULL

-- This will succeed
INSERT INTO employees (emp_id, emp_name, email) 
VALUES (1, 'John Doe', 'john@example.com');
```

### 2. UNIQUE Constraint

Ensures all values in a column or group of columns are different.

```sql
CREATE TABLE employees (
    emp_id INTEGER PRIMARY KEY,
    emp_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    social_security VARCHAR(11)
);

-- Named unique constraint
ALTER TABLE employees 
ADD CONSTRAINT uq_employees_ssn UNIQUE (social_security);

-- Multi-column unique constraint
CREATE TABLE project_assignments (
    emp_id INTEGER,
    project_id INTEGER,
    CONSTRAINT uq_assignment UNIQUE (emp_id, project_id)
);
```

### 3. PRIMARY KEY Constraint

Uniquely identifies each row in a table. A table can have only one primary key.

```sql
-- Single column primary key
CREATE TABLE departments (
    dept_id INTEGER PRIMARY KEY,
    dept_name VARCHAR(50) NOT NULL
);

-- Composite primary key
CREATE TABLE order_items (
    order_id INTEGER,
    line_item INTEGER,
    product_id INTEGER,
    quantity INTEGER,
    PRIMARY KEY (order_id, line_item)
);

-- Named primary key constraint
CREATE TABLE products (
    product_id INTEGER,
    product_name VARCHAR(100),
    CONSTRAINT pk_products PRIMARY KEY (product_id)
);
```

### 4. FOREIGN KEY Constraint

Creates a link between two tables and enforces referential integrity.

```sql
-- Create parent table
CREATE TABLE departments (
    dept_id INTEGER PRIMARY KEY,
    dept_name VARCHAR(50) NOT NULL
);

-- Create child table with foreign key
CREATE TABLE employees (
    emp_id INTEGER PRIMARY KEY,
    emp_name VARCHAR(50) NOT NULL,
    dept_id INTEGER,
    CONSTRAINT fk_emp_dept 
        FOREIGN KEY (dept_id) 
        REFERENCES departments(dept_id)
);

-- Foreign key with cascade options
CREATE TABLE orders (
    order_id INTEGER PRIMARY KEY,
    customer_id INTEGER,
    CONSTRAINT fk_order_customer 
        FOREIGN KEY (customer_id) 
        REFERENCES customers(customer_id)
        ON DELETE CASCADE
        ON UPDATE CASCADE
);
```

#### Foreign Key Actions

- **NO ACTION** (default): Prevents delete/update if referenced
- **CASCADE**: Automatically delete/update child records
- **SET NULL**: Set foreign key to NULL
- **SET DEFAULT**: Set foreign key to default value

### 5. CHECK Constraint

Validates data based on a logical expression.

```sql
CREATE TABLE employees (
    emp_id INTEGER PRIMARY KEY,
    emp_name VARCHAR(50) NOT NULL,
    salary NUMERIC(10,2) CHECK (salary > 0),
    age INTEGER CHECK (age >= 18 AND age <= 65),
    email VARCHAR(100) CHECK (email LIKE '%@%.%')
);

-- Named check constraint
ALTER TABLE employees 
ADD CONSTRAINT chk_salary_range 
CHECK (salary BETWEEN 20000 AND 200000);

-- Table-level check constraint
CREATE TABLE orders (
    order_id INTEGER PRIMARY KEY,
    order_date DATE NOT NULL,
    ship_date DATE,
    CONSTRAINT chk_dates CHECK (ship_date >= order_date)
);
```

### 6. DEFAULT Constraint

Provides a default value when no value is specified during insertion.

```sql
CREATE TABLE employees (
    emp_id INTEGER PRIMARY KEY,
    emp_name VARCHAR(50) NOT NULL,
    hire_date DATE DEFAULT CURRENT_DATE,
    status VARCHAR(20) DEFAULT 'ACTIVE',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    is_manager BOOLEAN DEFAULT FALSE
);

-- Using sequences as defaults
CREATE SEQUENCE seq_employee_id;

CREATE TABLE employees (
    emp_id INTEGER DEFAULT NEXT VALUE FOR seq_employee_id,
    emp_name VARCHAR(50) NOT NULL
);
```

## Adding Constraints to Existing Tables

```sql
-- Add NOT NULL constraint
ALTER TABLE employees ALTER COLUMN email SET NOT NULL;

-- Add UNIQUE constraint
ALTER TABLE employees ADD CONSTRAINT uq_email UNIQUE (email);

-- Add CHECK constraint
ALTER TABLE employees 
ADD CONSTRAINT chk_positive_salary CHECK (salary > 0);

-- Add FOREIGN KEY constraint
ALTER TABLE employees 
ADD CONSTRAINT fk_dept 
FOREIGN KEY (dept_id) REFERENCES departments(dept_id);

-- Add DEFAULT value
ALTER TABLE employees 
ALTER COLUMN status SET DEFAULT 'ACTIVE';
```

## Dropping Constraints

```sql
-- Drop constraint by name
ALTER TABLE employees DROP CONSTRAINT uq_email;
ALTER TABLE employees DROP CONSTRAINT fk_dept;

-- Drop NOT NULL constraint
ALTER TABLE employees ALTER COLUMN phone DROP NOT NULL;

-- Drop DEFAULT value
ALTER TABLE employees ALTER COLUMN status DROP DEFAULT;
```

## Viewing Constraints

```sql
-- Show all constraints for a table
SHOW TABLE employees;

-- Query system tables for constraints
SELECT 
    rc.RDB$CONSTRAINT_NAME AS constraint_name,
    rc.RDB$CONSTRAINT_TYPE AS constraint_type,
    rc.RDB$RELATION_NAME AS table_name
FROM RDB$RELATION_CONSTRAINTS rc
WHERE rc.RDB$RELATION_NAME = 'EMPLOYEES'
ORDER BY rc.RDB$CONSTRAINT_TYPE;

-- Show check constraints
SELECT 
    cc.RDB$CONSTRAINT_NAME,
    cc.RDB$TRIGGER_SOURCE
FROM RDB$CHECK_CONSTRAINTS cc
JOIN RDB$RELATION_CONSTRAINTS rc
    ON cc.RDB$CONSTRAINT_NAME = rc.RDB$CONSTRAINT_NAME
WHERE rc.RDB$RELATION_NAME = 'EMPLOYEES';
```

## Best Practices

1. **Name your constraints**: Use meaningful names for easier maintenance
   ```sql
   CONSTRAINT pk_employees PRIMARY KEY (emp_id)
   CONSTRAINT fk_emp_dept FOREIGN KEY (dept_id) REFERENCES departments(dept_id)
   CONSTRAINT chk_salary CHECK (salary > 0)
   ```

2. **Use appropriate constraint types**: 
   - PRIMARY KEY for unique row identification
   - FOREIGN KEY for relationships
   - CHECK for business rules
   - NOT NULL for required fields

3. **Consider performance**: 
   - Constraints add overhead during DML operations
   - Foreign keys create implicit indexes
   - CHECK constraints are evaluated for each row

4. **Plan for data migration**: 
   - Disable constraints during bulk loads if needed
   - Re-enable and validate after loading

## Examples: Complete Table with All Constraints

```sql
-- Create a comprehensive example
CREATE TABLE employees (
    -- Primary key with auto-increment
    emp_id INTEGER GENERATED BY DEFAULT AS IDENTITY PRIMARY KEY,
    
    -- NOT NULL constraints
    first_name VARCHAR(30) NOT NULL,
    last_name VARCHAR(30) NOT NULL,
    
    -- UNIQUE constraint
    email VARCHAR(100) NOT NULL UNIQUE,
    
    -- CHECK constraints
    salary NUMERIC(10,2) CHECK (salary > 0),
    hire_date DATE NOT NULL CHECK (hire_date <= CURRENT_DATE),
    
    -- DEFAULT constraint
    status VARCHAR(20) DEFAULT 'ACTIVE' NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP NOT NULL,
    
    -- Foreign key
    dept_id INTEGER,
    manager_id INTEGER,
    
    -- Named constraints
    CONSTRAINT chk_status CHECK (status IN ('ACTIVE', 'INACTIVE', 'TERMINATED')),
    CONSTRAINT fk_dept FOREIGN KEY (dept_id) REFERENCES departments(dept_id),
    CONSTRAINT fk_manager FOREIGN KEY (manager_id) REFERENCES employees(emp_id)
);
```

## Deferred Constraints

Firebird doesn't support deferred constraint checking within transactions, but you can work around this:

```sql
-- Temporarily disable/enable constraints
ALTER TABLE employees ALTER CONSTRAINT fk_manager INACTIVE;
-- Perform operations
ALTER TABLE employees ALTER CONSTRAINT fk_manager ACTIVE;
```

## Next Steps

- Learn about indexes and their relationship with constraints
- Understand triggers for complex validation
- Explore domains for reusable constraint definitions
- Study transaction isolation and constraint behavior
