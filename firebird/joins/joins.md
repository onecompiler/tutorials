# Joins in Firebird

Joins are used to retrieve data from multiple tables based on relationships between them. Firebird supports all standard SQL join types, allowing you to combine data effectively.

## Types of Joins

Firebird supports the following join types:
1. INNER JOIN
2. LEFT [OUTER] JOIN
3. RIGHT [OUTER] JOIN
4. FULL [OUTER] JOIN
5. CROSS JOIN
6. NATURAL JOIN

## Sample Tables

Let's create sample tables to demonstrate joins:

```sql
-- Create and populate departments table
CREATE TABLE departments (
    dept_id INTEGER PRIMARY KEY,
    dept_name VARCHAR(50),
    location VARCHAR(50)
);

INSERT INTO departments VALUES (1, 'Sales', 'New York');
INSERT INTO departments VALUES (2, 'IT', 'San Francisco');
INSERT INTO departments VALUES (3, 'HR', 'Chicago');
INSERT INTO departments VALUES (4, 'Marketing', 'Boston');

-- Create and populate employees table
CREATE TABLE employees (
    emp_id INTEGER PRIMARY KEY,
    emp_name VARCHAR(50),
    dept_id INTEGER,
    salary NUMERIC(10,2),
    hire_date DATE
);

INSERT INTO employees VALUES (101, 'John Smith', 1, 55000, '2020-01-15');
INSERT INTO employees VALUES (102, 'Jane Doe', 2, 75000, '2019-03-20');
INSERT INTO employees VALUES (103, 'Bob Johnson', 1, 60000, '2021-06-01');
INSERT INTO employees VALUES (104, 'Alice Brown', 3, 50000, '2020-11-10');
INSERT INTO employees VALUES (105, 'Charlie Wilson', NULL, 65000, '2022-02-28');
INSERT INTO employees VALUES (106, 'Diana Lee', 2, 80000, '2018-09-15');
```

## 1. INNER JOIN

Returns only rows that have matching values in both tables.

### Syntax
```sql
SELECT columns
FROM table1
INNER JOIN table2 ON table1.column = table2.column;
```

### Example
```sql
SELECT 
    e.emp_name,
    e.salary,
    d.dept_name,
    d.location
FROM employees e
INNER JOIN departments d ON e.dept_id = d.dept_id;
```

### Result
| emp_name | salary | dept_name | location |
|----------|---------|-----------|-----------|
| John Smith | 55000 | Sales | New York |
| Jane Doe | 75000 | IT | San Francisco |
| Bob Johnson | 60000 | Sales | New York |
| Alice Brown | 50000 | HR | Chicago |
| Diana Lee | 80000 | IT | San Francisco |

Note: Charlie Wilson is not included because dept_id is NULL.

## 2. LEFT OUTER JOIN

Returns all rows from the left table and matched rows from the right table. Non-matching rows show NULL for right table columns.

### Syntax
```sql
SELECT columns
FROM table1
LEFT [OUTER] JOIN table2 ON table1.column = table2.column;
```

### Example
```sql
SELECT 
    e.emp_name,
    e.salary,
    d.dept_name,
    d.location
FROM employees e
LEFT JOIN departments d ON e.dept_id = d.dept_id
ORDER BY e.emp_id;
```

### Result
| emp_name | salary | dept_name | location |
|----------|---------|-----------|-----------|
| John Smith | 55000 | Sales | New York |
| Jane Doe | 75000 | IT | San Francisco |
| Bob Johnson | 60000 | Sales | New York |
| Alice Brown | 50000 | HR | Chicago |
| Charlie Wilson | 65000 | NULL | NULL |
| Diana Lee | 80000 | IT | San Francisco |

## 3. RIGHT OUTER JOIN

Returns all rows from the right table and matched rows from the left table. Non-matching rows show NULL for left table columns.

### Example
```sql
SELECT 
    e.emp_name,
    e.salary,
    d.dept_name,
    d.location
FROM employees e
RIGHT JOIN departments d ON e.dept_id = d.dept_id
ORDER BY d.dept_id;
```

### Result
| emp_name | salary | dept_name | location |
|----------|---------|-----------|-----------|
| John Smith | 55000 | Sales | New York |
| Bob Johnson | 60000 | Sales | New York |
| Jane Doe | 75000 | IT | San Francisco |
| Diana Lee | 80000 | IT | San Francisco |
| Alice Brown | 50000 | HR | Chicago |
| NULL | NULL | Marketing | Boston |

## 4. FULL OUTER JOIN

Returns all rows from both tables, with NULL values where there's no match.

### Example
```sql
SELECT 
    e.emp_name,
    e.dept_id as emp_dept_id,
    d.dept_id as dept_dept_id,
    d.dept_name
FROM employees e
FULL OUTER JOIN departments d ON e.dept_id = d.dept_id
ORDER BY COALESCE(e.dept_id, d.dept_id);
```

### Result
| emp_name | emp_dept_id | dept_dept_id | dept_name |
|----------|-------------|--------------|-----------|
| John Smith | 1 | 1 | Sales |
| Bob Johnson | 1 | 1 | Sales |
| Jane Doe | 2 | 2 | IT |
| Diana Lee | 2 | 2 | IT |
| Alice Brown | 3 | 3 | HR |
| NULL | NULL | 4 | Marketing |
| Charlie Wilson | NULL | NULL | NULL |

## 5. CROSS JOIN

Returns the Cartesian product of both tables (every row from the first table combined with every row from the second table).

### Example
```sql
-- Simple cross join
SELECT 
    d.dept_name,
    e.emp_name
FROM departments d
CROSS JOIN employees e
ORDER BY d.dept_name, e.emp_name;
```

This produces 24 rows (4 departments × 6 employees).

### Practical Example: Generate Date-Employee Combinations
```sql
-- Create a dates table
CREATE TABLE report_dates (
    report_date DATE
);

INSERT INTO report_dates VALUES ('2024-01-01');
INSERT INTO report_dates VALUES ('2024-01-02');

-- Cross join to create all date-employee combinations
SELECT 
    r.report_date,
    e.emp_name
FROM report_dates r
CROSS JOIN employees e
ORDER BY r.report_date, e.emp_name;
```

## 6. Self Joins

Joining a table with itself, useful for hierarchical data.

### Example: Employee-Manager Relationship
```sql
-- Add manager_id to employees
ALTER TABLE employees ADD manager_id INTEGER;

UPDATE employees SET manager_id = 102 WHERE emp_id IN (101, 103);
UPDATE employees SET manager_id = 106 WHERE emp_id IN (104, 105);

-- Self join to show employees with their managers
SELECT 
    e.emp_name AS employee,
    m.emp_name AS manager
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.emp_id
ORDER BY e.emp_id;
```

## Multiple Joins

Combining multiple tables in a single query:

```sql
-- Create projects table
CREATE TABLE projects (
    project_id INTEGER PRIMARY KEY,
    project_name VARCHAR(50),
    dept_id INTEGER
);

INSERT INTO projects VALUES (1, 'Website Redesign', 1);
INSERT INTO projects VALUES (2, 'Database Migration', 2);
INSERT INTO projects VALUES (3, 'Employee Portal', 3);

-- Join three tables
SELECT 
    e.emp_name,
    d.dept_name,
    p.project_name
FROM employees e
INNER JOIN departments d ON e.dept_id = d.dept_id
INNER JOIN projects p ON d.dept_id = p.dept_id
ORDER BY e.emp_name;
```

## Join Conditions

### Using Multiple Conditions
```sql
SELECT *
FROM employees e
INNER JOIN departments d 
    ON e.dept_id = d.dept_id 
    AND e.salary > 60000;
```

### Using Non-Equality Conditions
```sql
-- Find employees who earn more than average in their department
SELECT 
    e1.emp_name,
    e1.salary,
    e1.dept_id
FROM employees e1
INNER JOIN (
    SELECT dept_id, AVG(salary) as avg_salary
    FROM employees
    WHERE dept_id IS NOT NULL
    GROUP BY dept_id
) e2 ON e1.dept_id = e2.dept_id AND e1.salary > e2.avg_salary;
```

## Performance Considerations

1. **Use indexes on join columns**: Create indexes on columns used in join conditions
   ```sql
   CREATE INDEX idx_emp_dept ON employees(dept_id);
   ```

2. **Join order matters**: Firebird's optimizer usually handles this, but you can influence it
   ```sql
   -- Force join order with +0
   SELECT *
   FROM employees e
   INNER JOIN departments d ON e.dept_id+0 = d.dept_id;
   ```

3. **Use appropriate join types**: Don't use OUTER JOIN when INNER JOIN suffices

4. **Filter early**: Apply WHERE conditions to reduce the dataset
   ```sql
   SELECT *
   FROM employees e
   INNER JOIN departments d ON e.dept_id = d.dept_id
   WHERE e.salary > 60000  -- Filter reduces join workload
   ```

## Common Join Patterns

### 1. Finding Unmatched Records
```sql
-- Employees without departments
SELECT e.*
FROM employees e
LEFT JOIN departments d ON e.dept_id = d.dept_id
WHERE d.dept_id IS NULL;

-- Departments without employees
SELECT d.*
FROM departments d
LEFT JOIN employees e ON d.dept_id = e.dept_id
WHERE e.emp_id IS NULL;
```

### 2. Aggregate with Joins
```sql
-- Department salary summary
SELECT 
    d.dept_name,
    COUNT(e.emp_id) as employee_count,
    AVG(e.salary) as avg_salary,
    SUM(e.salary) as total_salary
FROM departments d
LEFT JOIN employees e ON d.dept_id = e.dept_id
GROUP BY d.dept_name
ORDER BY d.dept_name;
```

### 3. EXISTS Alternative
```sql
-- Using JOIN instead of EXISTS
SELECT DISTINCT d.*
FROM departments d
INNER JOIN employees e ON d.dept_id = e.dept_id
WHERE e.salary > 70000;
```

## Best Practices

1. **Use meaningful aliases**: Makes queries more readable
2. **Specify join columns explicitly**: Avoid NATURAL JOIN in production
3. **Consider NULL values**: Especially important with OUTER JOINs
4. **Test with sample data**: Verify join results match expectations
5. **Monitor performance**: Use query plans to optimize joins

## Next Steps

- Learn about subqueries and correlated queries
- Explore Common Table Expressions (CTEs)
- Understand query optimization and indexes
- Practice with complex multi-table scenarios