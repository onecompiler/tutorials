
# Views in Firebird

A view is a virtual table based on the result of a SQL query. It doesn't store data physically but provides a way to simplify complex queries, enhance security, and present data in different ways without duplicating it.

## Why Use Views?

- **Simplify complex queries**: Encapsulate complex joins and calculations
- **Security**: Restrict access to specific columns or rows
- **Data abstraction**: Hide underlying table structure
- **Consistency**: Ensure everyone uses the same query logic
- **Backward compatibility**: Maintain old interfaces when tables change

## Creating Views

### Basic Syntax

```sql
CREATE VIEW view_name [(column_list)]
AS
    select_statement
[WITH CHECK OPTION];
```

### Simple View Example

```sql
-- Create a view showing active employees
CREATE VIEW v_active_employees AS
SELECT 
    emp_id,
    emp_name,
    dept_id,
    salary,
    hire_date
FROM employees
WHERE status = 'ACTIVE';

-- Use the view
SELECT * FROM v_active_employees WHERE dept_id = 10;
```

### View with Column Aliases

```sql
CREATE VIEW v_employee_summary 
    (id, full_name, department, annual_salary) AS
SELECT 
    e.emp_id,
    e.first_name || ' ' || e.last_name,
    d.dept_name,
    e.salary * 12
FROM employees e
JOIN departments d ON e.dept_id = d.dept_id;
```

## Types of Views

### 1. Simple Views

Based on a single table with no functions or groups:

```sql
CREATE VIEW v_customer_contacts AS
SELECT 
    customer_id,
    customer_name,
    email,
    phone
FROM customers;
```

### 2. Complex Views

Involve multiple tables, joins, functions, or grouping:

```sql
CREATE VIEW v_department_stats AS
SELECT 
    d.dept_id,
    d.dept_name,
    COUNT(e.emp_id) as employee_count,
    AVG(e.salary) as avg_salary,
    MIN(e.salary) as min_salary,
    MAX(e.salary) as max_salary
FROM departments d
LEFT JOIN employees e ON d.dept_id = e.dept_id
GROUP BY d.dept_id, d.dept_name;
```

### 3. Views with Subqueries

```sql
CREATE VIEW v_above_average_employees AS
SELECT 
    emp_id,
    emp_name,
    salary,
    dept_id
FROM employees e
WHERE salary > (
    SELECT AVG(salary) 
    FROM employees 
    WHERE dept_id = e.dept_id
);
```

## Updatable Views

Some views can be used for INSERT, UPDATE, and DELETE operations.

### Requirements for Updatable Views

A view is updatable if:
- Based on a single table
- No aggregate functions or GROUP BY
- No DISTINCT, UNION, or subqueries
- Includes all NOT NULL columns without defaults

```sql
-- Updatable view
CREATE VIEW v_employee_basic AS
SELECT 
    emp_id,
    emp_name,
    dept_id,
    salary
FROM employees;

-- Can update through this view
UPDATE v_employee_basic 
SET salary = salary * 1.1 
WHERE emp_id = 100;

-- Can insert through this view (if all required columns are included)
INSERT INTO v_employee_basic (emp_id, emp_name, dept_id, salary)
VALUES (200, 'New Employee', 10, 50000);
```

### WITH CHECK OPTION

Ensures that data modifications through the view satisfy the view's WHERE clause:

```sql
CREATE VIEW v_it_employees AS
SELECT * FROM employees
WHERE dept_id = (SELECT dept_id FROM departments WHERE dept_name = 'IT')
WITH CHECK OPTION;

-- This will fail because it violates the view's WHERE clause
UPDATE v_it_employees 
SET dept_id = 20 
WHERE emp_id = 100;  -- Error: CHECK OPTION violation
```

## Managing Views

### Altering Views

Firebird uses CREATE OR ALTER syntax:

```sql
CREATE OR ALTER VIEW v_employee_summary AS
SELECT 
    e.emp_id,
    e.first_name || ' ' || e.last_name as full_name,
    d.dept_name,
    e.salary,
    e.hire_date
FROM employees e
LEFT JOIN departments d ON e.dept_id = d.dept_id
WHERE e.status = 'ACTIVE';
```

### Dropping Views

```sql
-- Drop a single view
DROP VIEW v_employee_summary;

-- Check if view exists before dropping (Firebird 3.0+)
EXECUTE BLOCK
AS
BEGIN
    IF (EXISTS(SELECT 1 FROM RDB$RELATIONS 
               WHERE RDB$RELATION_NAME = 'V_EMPLOYEE_SUMMARY' 
               AND RDB$VIEW_BLR IS NOT NULL)) THEN
        EXECUTE STATEMENT 'DROP VIEW V_EMPLOYEE_SUMMARY';
END
```

### Viewing View Information

```sql
-- List all views
SELECT 
    RDB$RELATION_NAME AS view_name
FROM RDB$RELATIONS
WHERE RDB$VIEW_BLR IS NOT NULL
  AND RDB$SYSTEM_FLAG = 0
ORDER BY RDB$RELATION_NAME;

-- View definition
SELECT 
    RDB$VIEW_SOURCE
FROM RDB$RELATIONS
WHERE RDB$RELATION_NAME = 'V_EMPLOYEE_SUMMARY';

-- View dependencies
SELECT 
    d.RDB$DEPENDENT_NAME AS view_name,
    d.RDB$DEPENDED_ON_NAME AS depends_on,
    d.RDB$DEPENDENT_TYPE,
    d.RDB$DEPENDED_ON_TYPE
FROM RDB$DEPENDENCIES d
WHERE d.RDB$DEPENDENT_NAME = 'V_EMPLOYEE_SUMMARY'
ORDER BY d.RDB$DEPENDED_ON_NAME;
```

## Advanced View Examples

### 1. Hierarchical Data View

```sql
-- View showing employee hierarchy
CREATE VIEW v_employee_hierarchy AS
WITH RECURSIVE hierarchy AS (
    -- Anchor: top-level employees (no manager)
    SELECT 
        emp_id,
        emp_name,
        manager_id,
        0 as level,
        CAST(emp_name AS VARCHAR(500)) as path
    FROM employees
    WHERE manager_id IS NULL
    
    UNION ALL
    
    -- Recursive: employees with managers
    SELECT 
        e.emp_id,
        e.emp_name,
        e.manager_id,
        h.level + 1,
        h.path || ' > ' || e.emp_name
    FROM employees e
    JOIN hierarchy h ON e.manager_id = h.emp_id
)
SELECT * FROM hierarchy
ORDER BY path;
```

### 2. Pivot-Style View

```sql
-- View showing department salaries by month
CREATE VIEW v_dept_salary_by_month AS
SELECT 
    d.dept_name,
    EXTRACT(MONTH FROM p.payment_date) as month,
    EXTRACT(YEAR FROM p.payment_date) as year,
    SUM(p.amount) as total_salary
FROM departments d
JOIN employees e ON d.dept_id = e.dept_id
JOIN salary_payments p ON e.emp_id = p.emp_id
GROUP BY d.dept_name, 
         EXTRACT(YEAR FROM p.payment_date),
         EXTRACT(MONTH FROM p.payment_date);
```

### 3. Security View

```sql
-- View hiding sensitive salary information
CREATE VIEW v_employee_public AS
SELECT 
    emp_id,
    emp_name,
    dept_id,
    CASE 
        WHEN salary < 50000 THEN 'Junior'
        WHEN salary < 80000 THEN 'Mid-level'
        ELSE 'Senior'
    END as salary_grade,
    hire_date
FROM employees;

-- Grant access to the view, not the underlying table
GRANT SELECT ON v_employee_public TO public_user;
```

## Performance Considerations

### 1. View Complexity

```sql
-- Views are expanded at runtime
-- This query:
SELECT * FROM v_complex_view WHERE condition = 'X';

-- Is internally processed as:
SELECT * FROM (
    -- The entire view definition
) WHERE condition = 'X';
```

### 2. Indexed Views (Materialized Views Alternative)

Firebird doesn't have materialized views, but you can simulate them:

```sql
-- Create a table to store view results
CREATE TABLE mv_department_stats (
    dept_id INTEGER PRIMARY KEY,
    dept_name VARCHAR(50),
    employee_count INTEGER,
    avg_salary NUMERIC(10,2),
    last_updated TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Procedure to refresh the "materialized view"
CREATE PROCEDURE refresh_mv_department_stats
AS
BEGIN
    DELETE FROM mv_department_stats;
    
    INSERT INTO mv_department_stats (dept_id, dept_name, employee_count, avg_salary)
    SELECT 
        d.dept_id,
        d.dept_name,
        COUNT(e.emp_id),
        AVG(e.salary)
    FROM departments d
    LEFT JOIN employees e ON d.dept_id = e.dept_id
    GROUP BY d.dept_id, d.dept_name;
END;
```

## Best Practices

1. **Naming Convention**: Use prefixes like `v_` or `vw_` for views
2. **Documentation**: Comment complex views
   ```sql
   COMMENT ON VIEW v_employee_summary IS 
   'Provides employee summary with department information for reporting';
   ```

3. **Avoid Nested Views**: Don't create views based on other views
4. **Keep It Simple**: Complex views can hurt performance
5. **Security**: Use views to implement row-level security
6. **Testing**: Test view performance with EXPLAIN PLAN

## Common Use Cases

### 1. Data Denormalization

```sql
CREATE VIEW v_order_details AS
SELECT 
    o.order_id,
    o.order_date,
    c.customer_name,
    c.customer_email,
    p.product_name,
    oi.quantity,
    oi.unit_price,
    oi.quantity * oi.unit_price as line_total
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
JOIN order_items oi ON o.order_id = oi.order_id
JOIN products p ON oi.product_id = p.product_id;
```

### 2. Current Data Views

```sql
-- View showing only current year data
CREATE VIEW v_current_year_sales AS
SELECT * FROM sales
WHERE EXTRACT(YEAR FROM sale_date) = EXTRACT(YEAR FROM CURRENT_DATE);
```

### 3. Union Views

```sql
-- Combine data from multiple similar tables
CREATE VIEW v_all_transactions AS
SELECT 'Sale' as trans_type, sale_id as trans_id, amount, trans_date
FROM sales
UNION ALL
SELECT 'Refund', refund_id, -amount, trans_date
FROM refunds
UNION ALL
SELECT 'Adjustment', adj_id, amount, trans_date
FROM adjustments;
```

## Next Steps

- Learn about triggers on views
- Understand view security and permissions
- Explore Common Table Expressions (CTEs) as alternatives
- Study query optimization for complex views