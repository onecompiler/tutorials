# Reading Data

The SELECT statement is used to retrieve data from Cassandra tables. Understanding how to query data effectively in Cassandra is crucial, as it has different limitations compared to traditional SQL databases due to its distributed nature.

## Basic SELECT Syntax

```sql
SELECT column_list
FROM [keyspace_name.]table_name
[WHERE condition]
[ORDER BY column_name [ASC|DESC]]
[LIMIT n]
[ALLOW FILTERING];
```

## Reading All Data

### Select All Columns

```sql
SELECT * FROM student;
```

### Select with Keyspace

```sql
SELECT * FROM tutorial_db.student;
```

## Reading Specific Columns

```sql
SELECT id, name, city FROM student;
```

## Filtering Data with WHERE

### Primary Key Queries

Cassandra is optimized for queries using the primary key:

```sql
-- Query by partition key
SELECT * FROM student WHERE id = 2;

-- Query by partition key and clustering column
SELECT * FROM orders WHERE user_id = 123 AND order_date = '2024-01-15';
```

### Using IN Clause

```sql
SELECT * FROM student WHERE id IN (1, 2, 3);
```

### Range Queries on Clustering Columns

```sql
SELECT * FROM time_series_data 
WHERE sensor_id = 'sensor-001' 
AND timestamp >= '2024-01-01' 
AND timestamp < '2024-02-01';
```

## Ordering Results

ORDER BY can only be used with clustering columns:

```sql
SELECT * FROM events 
WHERE user_id = 123 
ORDER BY event_time DESC;
```

## Limiting Results

```sql
-- Get first 10 rows
SELECT * FROM student LIMIT 10;

-- Combine with WHERE
SELECT * FROM logs 
WHERE app_id = 'myapp' 
LIMIT 100;
```

## ALLOW FILTERING

Use with caution as it can be expensive:

```sql
-- Warning: This performs a full table scan
SELECT * FROM student 
WHERE city = 'New York' 
ALLOW FILTERING;
```

## Working with Collections

### Querying Lists

```sql
-- Check if list contains a value
SELECT * FROM user_interests 
WHERE user_id = 1 
AND interests CONTAINS 'coding';
```

### Querying Sets

```sql
-- Check if set contains a value
SELECT * FROM user_tags 
WHERE user_id = 1 
AND tags CONTAINS 'developer';
```

### Querying Maps

```sql
-- Query by map key
SELECT * FROM user_settings 
WHERE user_id = 1 
AND preferences['theme'] = 'dark';

-- Check if map contains a key
SELECT * FROM user_settings 
WHERE user_id = 1 
AND preferences CONTAINS KEY 'theme';
```

## JSON Output

Return results as JSON:

```sql
SELECT JSON * FROM student;

-- Specific columns as JSON
SELECT JSON id, name, city FROM student WHERE id = 1;
```

## Aggregate Functions

### COUNT

```sql
-- Count all rows
SELECT COUNT(*) FROM student;

-- Count with condition
SELECT COUNT(*) FROM orders WHERE user_id = 123;
```

### MIN, MAX, SUM, AVG

```sql
SELECT MAX(salary), MIN(salary), AVG(salary) 
FROM employees 
WHERE department = 'Engineering';
```

## Token Function

Used for pagination in large datasets:

```sql
-- Get partition token
SELECT token(id), id, name FROM student;

-- Pagination using token
SELECT * FROM student WHERE token(id) > token(100) LIMIT 10;
```

## System Functions

### TTL and WRITETIME

```sql
-- Get TTL of a column
SELECT TTL(session_data) FROM sessions WHERE session_id = 'abc123';

-- Get write timestamp
SELECT WRITETIME(last_login) FROM users WHERE user_id = 1;
```

### UUID Functions

```sql
-- Convert timeuuid to timestamp
SELECT dateOf(event_id), event_type FROM events LIMIT 5;

-- Get UUID from timeuuid
SELECT unixTimestampOf(created_at) FROM time_based_table;
```

## Performance Considerations

### Efficient Queries

```sql
-- Good: Uses partition key
SELECT * FROM users WHERE user_id = 123;

-- Good: Uses partition key + clustering key
SELECT * FROM posts WHERE user_id = 123 AND post_date = '2024-01-15';

-- Good: Range on clustering key
SELECT * FROM posts 
WHERE user_id = 123 
AND post_date >= '2024-01-01' 
AND post_date < '2024-02-01';
```

### Inefficient Queries

```sql
-- Bad: No partition key (requires ALLOW FILTERING)
SELECT * FROM users WHERE email = 'user@example.com' ALLOW FILTERING;

-- Bad: IN clause on partition key (multiple partitions)
SELECT * FROM users WHERE user_id IN (1,2,3,4,5,6,7,8,9,10);
```

## Best Practices

1. **Always include the partition key** in WHERE clause
2. **Avoid ALLOW FILTERING** in production
3. **Design tables based on query patterns**
4. **Use LIMIT to control result size**
5. **Be careful with IN clauses** - they can hit multiple partitions
6. **Consider using materialized views** for different query patterns

## Common Errors

### Missing Partition Key
```sql
-- Error: Cannot execute this query as it might involve data filtering
SELECT * FROM student WHERE name = 'John';

-- Fix: Add ALLOW FILTERING (not recommended) or redesign table
```

### Invalid ORDER BY
```sql
-- Error: ORDER BY is only supported on clustering columns
SELECT * FROM student ORDER BY name;

-- Fix: ORDER BY can only be used with clustering columns
```