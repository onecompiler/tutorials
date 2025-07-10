# Indexes in Firebird

Indexes are database structures that improve the speed of data retrieval operations on tables. They work like an index in a book - instead of reading every page to find information, you can look up the topic in the index and go directly to the relevant pages.

## Why Use Indexes?

- **Faster queries**: Speed up SELECT, UPDATE, and DELETE operations
- **Efficient sorting**: Improve ORDER BY performance
- **Unique constraints**: Enforce uniqueness
- **Foreign key support**: Automatically created for foreign keys

## Types of Indexes in Firebird

### 1. B-Tree Indexes (Default)
Standard indexes that work well for most scenarios:
- Equality searches (=)
- Range searches (<, >, BETWEEN)
- Sorting operations
- Join operations

### 2. Ascending/Descending Indexes
Control the sort order of index entries.

### 3. Unique Indexes
Ensure all values in the indexed column(s) are unique.

### 4. Composite Indexes
Indexes on multiple columns.

## Creating Indexes

### Basic Index Creation

```sql
-- Simple index on a single column
CREATE INDEX idx_emp_name ON employees(emp_name);

-- Unique index
CREATE UNIQUE INDEX idx_emp_email ON employees(email);

-- Descending index
CREATE DESCENDING INDEX idx_salary_desc ON employees(salary);
```

### Composite Index

```sql
-- Index on multiple columns
CREATE INDEX idx_emp_dept_salary ON employees(dept_id, salary);

-- Mixed ascending/descending
CREATE INDEX idx_orders_composite 
ON orders(customer_id ASC, order_date DESC);
```

### Expression Index

```sql
-- Index on computed values
CREATE INDEX idx_upper_name ON employees
COMPUTED BY (UPPER(emp_name));

-- Index on substring
CREATE INDEX idx_email_domain ON users
COMPUTED BY (SUBSTRING(email FROM POSITION('@' IN email) + 1));
```

## Managing Indexes

### Viewing Indexes

```sql
-- Show all indexes for a table
SHOW INDEX employees;

-- Query system tables for index information
SELECT 
    i.RDB$INDEX_NAME AS index_name,
    i.RDB$RELATION_NAME AS table_name,
    i.RDB$UNIQUE_FLAG AS is_unique,
    i.RDB$INDEX_TYPE AS index_type,
    i.RDB$STATISTICS AS selectivity
FROM RDB$INDICES i
WHERE i.RDB$RELATION_NAME = 'EMPLOYEES'
  AND i.RDB$SYSTEM_FLAG = 0
ORDER BY i.RDB$INDEX_NAME;

-- Show index segments (columns)
SELECT 
    s.RDB$INDEX_NAME,
    s.RDB$FIELD_NAME,
    s.RDB$FIELD_POSITION
FROM RDB$INDEX_SEGMENTS s
WHERE s.RDB$INDEX_NAME LIKE 'IDX_%'
ORDER BY s.RDB$INDEX_NAME, s.RDB$FIELD_POSITION;
```

### Altering Indexes

```sql
-- Deactivate index (for maintenance)
ALTER INDEX idx_emp_name INACTIVE;

-- Reactivate index
ALTER INDEX idx_emp_name ACTIVE;

-- Rebuild index statistics
SET STATISTICS INDEX idx_emp_name;
```

### Dropping Indexes

```sql
-- Drop an index
DROP INDEX idx_emp_name;

-- Note: You cannot drop indexes created by constraints
-- You must drop the constraint instead
```

## Index Usage and Query Plans

### Viewing Query Plans

```sql
-- In isql, enable plan display
SET PLAN ON;

-- Example query
SELECT * FROM employees WHERE emp_name = 'John Doe';
-- PLAN (EMPLOYEES INDEX (IDX_EMP_NAME))

-- Natural scan (no index)
SELECT * FROM employees WHERE UPPER(emp_name) = 'JOHN DOE';
-- PLAN (EMPLOYEES NATURAL)
```

### Forcing Index Usage

```sql
-- Force specific index
SELECT * FROM employees 
WHERE emp_name = 'John Doe'
PLAN (EMPLOYEES INDEX (IDX_EMP_NAME));

-- Force natural scan (no index)
SELECT * FROM employees 
WHERE emp_name = 'John Doe'
PLAN (EMPLOYEES NATURAL);
```

## Index Selectivity

Selectivity indicates how well an index differentiates between rows:
- 0 = all values are the same (poor selectivity)
- 1 = all values are unique (excellent selectivity)

```sql
-- View index selectivity
SELECT 
    RDB$INDEX_NAME,
    RDB$STATISTICS AS selectivity
FROM RDB$INDICES
WHERE RDB$RELATION_NAME = 'EMPLOYEES'
  AND RDB$SYSTEM_FLAG = 0;

-- Recalculate selectivity
SET STATISTICS INDEX idx_emp_dept;
```

## Best Practices

### 1. Choose Columns Wisely

Good candidates for indexing:
- Primary key columns (automatic)
- Foreign key columns (automatic)
- Columns used in WHERE clauses
- Columns used in JOIN conditions
- Columns used in ORDER BY

Poor candidates:
- Columns with few distinct values (e.g., gender, status)
- Frequently updated columns
- Small tables (< 1000 rows)

### 2. Composite Index Order Matters

```sql
-- This index helps queries filtering by dept_id or (dept_id, salary)
CREATE INDEX idx_dept_salary ON employees(dept_id, salary);

-- These queries can use the index:
SELECT * FROM employees WHERE dept_id = 10;
SELECT * FROM employees WHERE dept_id = 10 AND salary > 50000;

-- This query cannot use the index efficiently:
SELECT * FROM employees WHERE salary > 50000;
```

### 3. Avoid Over-Indexing

Indexes have costs:
- Storage space
- Slower INSERT/UPDATE/DELETE operations
- Maintenance overhead

### 4. Monitor Index Usage

```sql
-- Check if indexes are being used
SELECT 
    mon$index_name,
    mon$record_seq_reads,  -- Sequential reads
    mon$record_idx_reads   -- Index reads
FROM mon$record_stats
WHERE mon$stat_group = 1  -- Database level
ORDER BY mon$record_idx_reads DESC;
```

## Common Indexing Patterns

### 1. Covering Index

Include all columns needed by a query:

```sql
-- Query needs emp_id, emp_name, and salary
CREATE INDEX idx_covering 
ON employees(dept_id, emp_id, emp_name, salary);

-- This query can be satisfied entirely from the index
SELECT emp_id, emp_name, salary 
FROM employees 
WHERE dept_id = 10;
```

### 2. Partial Index (Using Computed By)

```sql
-- Index only active employees
CREATE INDEX idx_active_employees
ON employees
COMPUTED BY (CASE WHEN status = 'ACTIVE' THEN emp_id ELSE NULL END);
```

### 3. Function-Based Index

```sql
-- Index for case-insensitive searches
CREATE INDEX idx_upper_email
ON employees
COMPUTED BY (UPPER(email));

-- Query can use this index
SELECT * FROM employees WHERE UPPER(email) = 'JOHN@EXAMPLE.COM';
```

## Index Maintenance

### Regular Maintenance Tasks

```sql
-- 1. Update statistics for all indexes
EXECUTE BLOCK
AS
DECLARE VARIABLE idx_name VARCHAR(31);
BEGIN
    FOR SELECT RDB$INDEX_NAME 
        FROM RDB$INDICES 
        WHERE RDB$SYSTEM_FLAG = 0
        INTO :idx_name
    DO
        EXECUTE STATEMENT 'SET STATISTICS INDEX ' || :idx_name;
END

-- 2. Rebuild indexes after bulk operations
ALTER INDEX idx_emp_name INACTIVE;
ALTER INDEX idx_emp_name ACTIVE;

-- 3. Monitor index health
SELECT 
    idx.RDB$INDEX_NAME,
    idx.RDB$STATISTICS,
    COUNT(*) as segment_count
FROM RDB$INDICES idx
JOIN RDB$INDEX_SEGMENTS seg 
    ON idx.RDB$INDEX_NAME = seg.RDB$INDEX_NAME
WHERE idx.RDB$SYSTEM_FLAG = 0
GROUP BY idx.RDB$INDEX_NAME, idx.RDB$STATISTICS
HAVING idx.RDB$STATISTICS < 0.01;  -- Poor selectivity
```

## Performance Tips

1. **Use indexes for large tables**: Tables with > 10,000 rows benefit most
2. **Index foreign keys**: If not automatically indexed
3. **Consider multi-column indexes**: For queries with multiple conditions
4. **Monitor slow queries**: Add indexes based on actual usage
5. **Review periodically**: Remove unused indexes

## Common Pitfalls

1. **Function calls prevent index usage**:
   ```sql
   -- Bad: Cannot use index
   SELECT * FROM employees WHERE UPPER(emp_name) = 'JOHN';
   
   -- Good: Create computed index
   CREATE INDEX idx_upper_name ON employees COMPUTED BY (UPPER(emp_name));
   ```

2. **Leading wildcards prevent index usage**:
   ```sql
   -- Bad: Cannot use index
   SELECT * FROM employees WHERE emp_name LIKE '%Smith';
   
   -- Good: Can use index
   SELECT * FROM employees WHERE emp_name LIKE 'Smith%';
   ```

3. **Data type mismatches**:
   ```sql
   -- Ensure consistent data types in comparisons
   SELECT * FROM employees WHERE emp_id = '123';  -- String vs Integer
   ```

## Next Steps

- Learn about query optimization
- Understand transaction impact on indexes
- Explore database statistics and monitoring
- Study index internals and page structure