# Table Operations

This tutorial covers all table-related operations in Cassandra, including creating, altering, and managing tables.

## Creating Tables

### Basic Syntax

```sql
CREATE TABLE [IF NOT EXISTS] [keyspace_name.]table_name (
    column_definition,
    column_definition,
    ...
    PRIMARY KEY (partition_key, [clustering_columns])
) [WITH table_options];
```

### Simple Table

```sql
CREATE TABLE users (
    user_id UUID PRIMARY KEY,
    username TEXT,
    email TEXT,
    created_at TIMESTAMP
);
```

### Compound Primary Key

```sql
CREATE TABLE user_activities (
    user_id UUID,
    activity_timestamp TIMESTAMP,
    activity_type TEXT,
    details TEXT,
    PRIMARY KEY (user_id, activity_timestamp)
) WITH CLUSTERING ORDER BY (activity_timestamp DESC);
```

### Composite Partition Key

```sql
CREATE TABLE sensor_data (
    sensor_type TEXT,
    sensor_id UUID,
    reading_time TIMESTAMP,
    temperature DOUBLE,
    humidity DOUBLE,
    PRIMARY KEY ((sensor_type, sensor_id), reading_time)
) WITH CLUSTERING ORDER BY (reading_time DESC);
```

## Primary Keys

### Partition Key

Determines data distribution:

```sql
-- Single partition key
PRIMARY KEY (user_id)

-- Composite partition key
PRIMARY KEY ((year, month))
```

### Clustering Columns

Sort data within partitions:

```sql
CREATE TABLE time_series (
    device_id UUID,
    timestamp TIMESTAMP,
    value DOUBLE,
    PRIMARY KEY (device_id, timestamp)
) WITH CLUSTERING ORDER BY (timestamp DESC);
```

## Table Options

### Clustering Order

```sql
CREATE TABLE events (
    user_id UUID,
    event_time TIMESTAMP,
    event_type TEXT,
    PRIMARY KEY (user_id, event_time)
) WITH CLUSTERING ORDER BY (event_time DESC);
```

### Compaction Strategy

```sql
CREATE TABLE large_table (
    id UUID PRIMARY KEY,
    data TEXT
) WITH compaction = {
    'class': 'LeveledCompactionStrategy',
    'sstable_size_in_mb': 160
};
```

### Compression

```sql
CREATE TABLE compressed_data (
    id UUID PRIMARY KEY,
    payload TEXT
) WITH compression = {
    'class': 'LZ4Compressor',
    'chunk_length_in_kb': 64
};
```

### Time To Live (TTL)

```sql
CREATE TABLE sessions (
    session_id UUID PRIMARY KEY,
    user_id UUID,
    data TEXT
) WITH default_time_to_live = 3600;  -- 1 hour TTL
```

### Caching Options

```sql
CREATE TABLE cached_data (
    id UUID PRIMARY KEY,
    data TEXT
) WITH caching = {
    'keys': 'ALL',
    'rows_per_partition': 'NONE'
};
```

## Altering Tables

### Add Columns

```sql
ALTER TABLE users ADD phone_number TEXT;
ALTER TABLE users ADD address frozen<map<text, text>>;
```

### Drop Columns

```sql
ALTER TABLE users DROP phone_number;
```

### Rename Columns

```sql
-- Note: Only works for non-primary key columns
ALTER TABLE users RENAME email TO email_address;
```

### Change Table Options

```sql
-- Change compaction strategy
ALTER TABLE users 
WITH compaction = {
    'class': 'SizeTieredCompactionStrategy'
};

-- Change caching
ALTER TABLE users 
WITH caching = {
    'keys': 'ALL',
    'rows_per_partition': '100'
};

-- Set TTL
ALTER TABLE sessions 
WITH default_time_to_live = 7200;
```

## Dropping Tables

```sql
-- Drop table
DROP TABLE users;

-- Drop table if exists
DROP TABLE IF EXISTS users;
```

## Truncating Tables

Remove all data but keep schema:

```sql
TRUNCATE TABLE users;
-- or
TRUNCATE users;
```

## Table Metadata

### Describe Table

```sql
-- Show table structure
DESCRIBE TABLE users;

-- Show create statement
SHOW CREATE TABLE users;
```

### List Tables

```sql
-- List all tables in current keyspace
DESCRIBE TABLES;

-- List tables in specific keyspace
DESCRIBE TABLES IN my_keyspace;
```

## Static Columns

Shared across all rows in a partition:

```sql
CREATE TABLE user_stats (
    user_id UUID,
    stat_date DATE,
    total_activities INT STATIC,  -- Same for all dates per user
    daily_activities INT,
    PRIMARY KEY (user_id, stat_date)
);

-- Update static column
UPDATE user_stats SET total_activities = 100 WHERE user_id = 123e4567-e89b-12d3-a456-426614174000;
```

## Secondary Indexes

### Create Index

```sql
-- Create index on regular column
CREATE INDEX ON users (email);

-- Create named index
CREATE INDEX users_email_idx ON users (email);

-- Create index on collection values
CREATE INDEX ON users (tags);  -- For SET/LIST
CREATE INDEX ON users (preferences) VALUES;  -- For MAP values
```

### Drop Index

```sql
DROP INDEX users_email_idx;
DROP INDEX IF EXISTS users_email_idx;
```

## Materialized Views

Create denormalized views for different query patterns:

```sql
-- Base table
CREATE TABLE users (
    user_id UUID,
    email TEXT,
    username TEXT,
    created_at TIMESTAMP,
    PRIMARY KEY (user_id)
);

-- Materialized view by email
CREATE MATERIALIZED VIEW users_by_email AS
    SELECT user_id, email, username, created_at
    FROM users
    WHERE email IS NOT NULL AND user_id IS NOT NULL
    PRIMARY KEY (email, user_id);
```

## Table Properties Reference

### All Table Options

```sql
CREATE TABLE comprehensive_example (
    id UUID PRIMARY KEY,
    data TEXT
) WITH bloom_filter_fp_chance = 0.01
    AND caching = {'keys': 'ALL', 'rows_per_partition': 'NONE'}
    AND comment = 'Example table with all options'
    AND compaction = {'class': 'SizeTieredCompactionStrategy'}
    AND compression = {'class': 'LZ4Compressor'}
    AND crc_check_chance = 1.0
    AND dclocal_read_repair_chance = 0.1
    AND default_time_to_live = 0
    AND gc_grace_seconds = 864000
    AND max_index_interval = 2048
    AND memtable_flush_period_in_ms = 0
    AND min_index_interval = 128
    AND read_repair_chance = 0.0
    AND speculative_retry = '99PERCENTILE';
```

## Best Practices

### Primary Key Design

1. **Choose partition key carefully**: Ensures even distribution
2. **Avoid large partitions**: Keep under 100MB
3. **Use clustering columns**: For time-series data
4. **Consider query patterns**: Design for your queries

### Table Design

1. **Denormalize data**: Duplicate data for query efficiency
2. **One table per query**: Design tables for specific access patterns
3. **Avoid secondary indexes**: In production, use materialized views
4. **Use appropriate data types**: Don't use TEXT for everything

### Performance Tips

1. **Use proper compaction strategy**:
   - SizeTiered: Write-heavy workloads
   - Leveled: Read-heavy workloads
   - TimeWindow: Time-series data

2. **Configure compression**: Reduces storage and I/O

3. **Set appropriate TTL**: For temporary data

4. **Monitor partition sizes**: Use nodetool to check

## Common Mistakes

### Mistake 1: Hot Partitions

```sql
-- Bad: All data goes to same partition
CREATE TABLE logs (
    log_level TEXT,
    timestamp TIMESTAMP,
    message TEXT,
    PRIMARY KEY (log_level, timestamp)
);

-- Good: Distribute across multiple partitions
CREATE TABLE logs (
    log_level TEXT,
    date DATE,
    timestamp TIMESTAMP,
    message TEXT,
    PRIMARY KEY ((log_level, date), timestamp)
);
```

### Mistake 2: Large Collections

```sql
-- Bad: Unbounded list growth
CREATE TABLE user_events (
    user_id UUID PRIMARY KEY,
    all_events LIST<TEXT>  -- Can grow without limit
);

-- Good: Use clustering columns
CREATE TABLE user_events (
    user_id UUID,
    event_time TIMESTAMP,
    event_data TEXT,
    PRIMARY KEY (user_id, event_time)
);
```

### Mistake 3: Too Many Secondary Indexes

```sql
-- Bad: Multiple secondary indexes
CREATE INDEX ON users (email);
CREATE INDEX ON users (phone);
CREATE INDEX ON users (city);

-- Good: Use materialized views for different access patterns
```