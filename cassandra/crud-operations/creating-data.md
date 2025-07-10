# Inserting Data

The INSERT command is used to add new rows of data into a Cassandra table. Unlike traditional SQL databases, Cassandra uses an upsert approach - if a row with the same primary key already exists, it will be overwritten.

## Syntax

```sql
INSERT INTO [keyspace_name.]table_name 
(column1, column2, column3, ...) 
VALUES (value1, value2, value3, ...)
[USING option];
```

## Basic Insert Examples

### Simple Insert

```sql
INSERT INTO student (id, name, city) 
VALUES (1, 'Paul', 'Texas');

INSERT INTO student (id, name, city) 
VALUES (2, 'John', 'New York');

INSERT INTO student (id, name, city) 
VALUES (3, 'Max', 'New Jersey');
```

### Insert with All Columns

```sql
INSERT INTO employee (emp_id, first_name, last_name, department, salary, hire_date)
VALUES (101, 'Alice', 'Johnson', 'Engineering', 75000, '2023-01-15');
```

## Insert Options

### Using TTL (Time To Live)

Set data to automatically expire after a specified time in seconds:

```sql
INSERT INTO session_data (session_id, user_id, data)
VALUES ('abc123', 1001, 'session info')
USING TTL 3600;  -- Data expires after 1 hour
```

### Using Timestamp

Specify a custom timestamp for the insert:

```sql
INSERT INTO events (event_id, event_type, description)
VALUES (uuid(), 'login', 'User logged in')
USING TIMESTAMP 1234567890;
```

### Combining TTL and Timestamp

```sql
INSERT INTO cache_data (key, value)
VALUES ('user:1001', 'cached data')
USING TTL 300 AND TIMESTAMP 1234567890;
```

## Working with Different Data Types

### UUID Values

```sql
INSERT INTO users (user_id, username, created_at)
VALUES (uuid(), 'newuser', toTimestamp(now()));
```

### Collections

```sql
-- List
INSERT INTO user_interests (user_id, interests)
VALUES (1, ['reading', 'gaming', 'coding']);

-- Set
INSERT INTO user_tags (user_id, tags)
VALUES (1, {'developer', 'gamer', 'reader'});

-- Map
INSERT INTO user_settings (user_id, preferences)
VALUES (1, {'theme': 'dark', 'language': 'en', 'notifications': 'on'});
```

### JSON Insert

Cassandra supports inserting data as JSON:

```sql
INSERT INTO users JSON '{"user_id": 1, "username": "john_doe", "email": "john@example.com"}';
```

## Conditional Inserts

### IF NOT EXISTS

Insert only if the row doesn't already exist:

```sql
INSERT INTO users (user_id, username, email)
VALUES (1, 'john_doe', 'john@example.com')
IF NOT EXISTS;
```

This returns a boolean indicating success or failure.

## Batch Inserts

For inserting multiple rows efficiently:

```sql
BEGIN BATCH
    INSERT INTO student (id, name, city) VALUES (4, 'Emma', 'Chicago');
    INSERT INTO student (id, name, city) VALUES (5, 'Oliver', 'Seattle');
    INSERT INTO student (id, name, city) VALUES (6, 'Sophia', 'Boston');
APPLY BATCH;
```

## Important Notes

1. **Primary Key Constraint**: You cannot insert a row without providing all primary key columns
2. **Upsert Behavior**: INSERT will overwrite existing rows with the same primary key
3. **No Foreign Key Checks**: Cassandra doesn't enforce referential integrity
4. **Timestamp**: Every insert has an associated timestamp used for conflict resolution
5. **Performance**: Cassandra is optimized for writes, making inserts very fast

## Common Errors and Solutions

### Missing Primary Key
```sql
-- Error: Primary key column 'id' is required
INSERT INTO student (name, city) VALUES ('Jane', 'Miami');

-- Correct:
INSERT INTO student (id, name, city) VALUES (7, 'Jane', 'Miami');
```

### Invalid Data Type
```sql
-- Error: Invalid data type for column
INSERT INTO student (id, name, city) VALUES ('abc', 'Jane', 'Miami');

-- Correct:
INSERT INTO student (id, name, city) VALUES (8, 'Jane', 'Miami');
```