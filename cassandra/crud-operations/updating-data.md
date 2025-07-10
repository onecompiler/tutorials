# Updating Data

The UPDATE statement in Cassandra is used to modify existing data or insert new data if it doesn't exist (upsert behavior). Understanding how updates work is crucial for maintaining data consistency.

## Basic Syntax

```sql
UPDATE [keyspace_name.]table_name
[USING option]
SET column1 = value1, column2 = value2, ...
WHERE primary_key_condition
[IF condition];
```

## Simple Updates

### Update Single Column

```sql
UPDATE student 
SET city = 'San Francisco' 
WHERE id = 1;
```

### Update Multiple Columns

```sql
UPDATE student 
SET name = 'John Smith', 
    city = 'New York',
    phone_number = '555-0123'
WHERE id = 2;
```

## Update Options

### Using TTL

Set Time To Live for specific columns:

```sql
UPDATE user_sessions 
USING TTL 3600  -- Expire in 1 hour
SET last_activity = toTimestamp(now()),
    session_data = 'active'
WHERE session_id = 'abc123';
```

### Using Timestamp

Specify custom timestamp for the update:

```sql
UPDATE events 
USING TIMESTAMP 1234567890
SET status = 'processed'
WHERE event_id = uuid();
```

### Combining TTL and Timestamp

```sql
UPDATE cache_entries 
USING TTL 300 AND TIMESTAMP 1234567890
SET cached_value = 'some data'
WHERE cache_key = 'user:123:profile';
```

## Updating Collections

### Lists

```sql
-- Append to list
UPDATE user_activities 
SET activities = activities + ['logged_in'] 
WHERE user_id = 123;

-- Prepend to list
UPDATE user_activities 
SET activities = ['new_activity'] + activities 
WHERE user_id = 123;

-- Update specific position
UPDATE user_activities 
SET activities[2] = 'updated_activity' 
WHERE user_id = 123;

-- Remove from list
UPDATE user_activities 
SET activities = activities - ['old_activity'] 
WHERE user_id = 123;
```

### Sets

```sql
-- Add to set
UPDATE user_tags 
SET tags = tags + {'new_tag', 'another_tag'} 
WHERE user_id = 123;

-- Remove from set
UPDATE user_tags 
SET tags = tags - {'old_tag'} 
WHERE user_id = 123;

-- Replace entire set
UPDATE user_tags 
SET tags = {'tag1', 'tag2', 'tag3'} 
WHERE user_id = 123;
```

### Maps

```sql
-- Add/Update map entries
UPDATE user_settings 
SET preferences = preferences + {'theme': 'dark', 'language': 'en'} 
WHERE user_id = 123;

-- Update specific key
UPDATE user_settings 
SET preferences['theme'] = 'light' 
WHERE user_id = 123;

-- Remove map entries
UPDATE user_settings 
SET preferences = preferences - {'old_key'} 
WHERE user_id = 123;

-- Delete specific key
DELETE preferences['unwanted_key'] FROM user_settings 
WHERE user_id = 123;
```

## Counter Updates

Counters have special update syntax:

```sql
-- Increment counter
UPDATE page_views 
SET view_count = view_count + 1 
WHERE page_id = 'home';

-- Decrement counter
UPDATE inventory 
SET quantity = quantity - 5 
WHERE product_id = 'ABC123';

-- Increment by multiple
UPDATE statistics 
SET total_visits = total_visits + 10 
WHERE stat_id = 'daily';
```

## Conditional Updates

### Update If Exists

```sql
UPDATE users 
SET email = 'newemail@example.com'
WHERE user_id = 123
IF EXISTS;
```

### Update with Condition

```sql
UPDATE accounts 
SET balance = 500
WHERE account_id = 123
IF balance = 600;
```

### Multiple Conditions

```sql
UPDATE products 
SET price = 29.99,
    last_updated = toTimestamp(now())
WHERE product_id = 'PROD123'
IF stock > 0 AND status = 'active';
```

## Batch Updates

### Logged Batch

```sql
BEGIN BATCH
    UPDATE users SET last_login = toTimestamp(now()) WHERE user_id = 123;
    UPDATE user_stats SET login_count = login_count + 1 WHERE user_id = 123;
    INSERT INTO audit_log (id, user_id, action) VALUES (uuid(), 123, 'login');
APPLY BATCH;
```

### Unlogged Batch

```sql
BEGIN UNLOGGED BATCH
    UPDATE inventory SET quantity = quantity - 1 WHERE product_id = 'A1';
    UPDATE inventory SET quantity = quantity - 2 WHERE product_id = 'B2';
    UPDATE inventory SET quantity = quantity - 3 WHERE product_id = 'C3';
APPLY BATCH;
```

## Static Column Updates

```sql
-- Update static column (affects all rows in partition)
UPDATE user_accounts 
SET account_status = 'premium'  -- static column
WHERE user_id = 123;
```

## JSON Updates

```sql
-- Update using JSON
UPDATE users 
SET email = 'john@example.com',
    phone = '555-0123'
WHERE user_id = 123;

-- Can also be written as:
INSERT INTO users JSON '{"user_id": 123, "email": "john@example.com", "phone": "555-0123"}';
```

## Performance Considerations

### Efficient Updates

```sql
-- Good: Update by primary key
UPDATE users SET status = 'active' WHERE user_id = 123;

-- Good: Update with partition key and clustering key
UPDATE orders SET status = 'shipped' 
WHERE user_id = 123 AND order_id = 456;
```

### Inefficient Updates

```sql
-- Bad: Cannot update without full primary key
UPDATE users SET status = 'active' WHERE email = 'user@example.com';
-- Error: PRIMARY KEY column "user_id" cannot be restricted

-- Bad: Cannot update primary key columns
UPDATE users SET user_id = 456 WHERE user_id = 123;
-- Error: PRIMARY KEY part user_id cannot be updated
```

## Best Practices

1. **Always include the complete primary key** in WHERE clause
2. **Use appropriate consistency levels** for critical updates
3. **Consider using TTL** for temporary data
4. **Be careful with collections** - they can grow unbounded
5. **Use conditional updates sparingly** - they use lightweight transactions
6. **Batch related updates** but keep batches small
7. **Monitor partition sizes** when updating collections

## Common Patterns

### Upsert Pattern

```sql
-- This inserts if doesn't exist, updates if exists
UPDATE users 
SET username = 'johndoe',
    email = 'john@example.com',
    updated_at = toTimestamp(now())
WHERE user_id = 123;
```

### Audit Trail Pattern

```sql
BEGIN BATCH
    UPDATE entities SET status = 'modified' WHERE id = 123;
    INSERT INTO audit_log (id, entity_id, action, timestamp) 
    VALUES (uuid(), 123, 'status_change', toTimestamp(now()));
APPLY BATCH;
```

### Soft Delete Pattern

```sql
UPDATE users 
USING TTL 2592000  -- 30 days
SET deleted = true,
    deleted_at = toTimestamp(now())
WHERE user_id = 123;
```

## Error Handling

Common update errors:
- **Invalid primary key**: Ensure all primary key columns are provided
- **Type mismatch**: Values must match column data types
- **Collection size limits**: Collections have size restrictions
- **Counter restrictions**: Counters can't be mixed with regular columns