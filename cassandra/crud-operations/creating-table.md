# Creating Tables

Tables in Cassandra store data in a structured format. Understanding how to create tables with proper primary keys and options is essential for building efficient Cassandra applications.

## Basic Syntax

```sql
CREATE TABLE [IF NOT EXISTS] [keyspace.]table_name (
    column1_name data_type,
    column2_name data_type,
    ...
    PRIMARY KEY (partition_key, [clustering_columns])
) [WITH table_options];
```

## Simple Table Example

```sql
CREATE TABLE student (
    id INT PRIMARY KEY,
    name TEXT,
    city TEXT,
    phone_number TEXT,
    enrollment_date DATE
);
```

## Primary Key Types

### Single Primary Key

```sql
CREATE TABLE users (
    user_id UUID PRIMARY KEY,
    username TEXT,
    email TEXT
);
```

### Compound Primary Key

```sql
CREATE TABLE user_posts (
    user_id UUID,
    post_id TIMEUUID,
    title TEXT,
    content TEXT,
    PRIMARY KEY (user_id, post_id)
) WITH CLUSTERING ORDER BY (post_id DESC);
```

In this example:
- `user_id` is the partition key
- `post_id` is the clustering column
- Data is sorted by `post_id` in descending order within each partition

### Composite Partition Key

```sql
CREATE TABLE sensor_readings (
    location TEXT,
    sensor_id INT,
    reading_time TIMESTAMP,
    temperature DOUBLE,
    humidity DOUBLE,
    PRIMARY KEY ((location, sensor_id), reading_time)
) WITH CLUSTERING ORDER BY (reading_time DESC);
```

Here `(location, sensor_id)` together form the partition key.

## Column Data Types

### Common Data Types

```sql
CREATE TABLE data_types_example (
    -- Numeric types
    tiny_num TINYINT,
    small_num SMALLINT,
    normal_int INT,
    big_num BIGINT,
    decimal_num DECIMAL,
    float_num FLOAT,
    double_num DOUBLE,
    
    -- Text types
    ascii_text ASCII,
    normal_text TEXT,
    varchar_text VARCHAR,
    
    -- Time types
    created_date DATE,
    created_time TIME,
    created_timestamp TIMESTAMP,
    
    -- Other types
    id_field UUID,
    time_id TIMEUUID,
    ip_addr INET,
    is_active BOOLEAN,
    data_blob BLOB,
    
    PRIMARY KEY (id_field)
);
```

### Collection Types

```sql
CREATE TABLE collections_example (
    user_id UUID PRIMARY KEY,
    emails SET<TEXT>,
    phone_numbers LIST<TEXT>,
    properties MAP<TEXT, TEXT>
);
```

## Table Options

### Clustering Order

```sql
CREATE TABLE time_series_data (
    device_id UUID,
    timestamp TIMESTAMP,
    value DOUBLE,
    PRIMARY KEY (device_id, timestamp)
) WITH CLUSTERING ORDER BY (timestamp DESC);
```

### Compaction Strategy

```sql
CREATE TABLE events (
    id UUID PRIMARY KEY,
    data TEXT
) WITH compaction = {
    'class': 'SizeTieredCompactionStrategy',
    'min_threshold': 4,
    'max_threshold': 32
};
```

### Compression

```sql
CREATE TABLE large_data (
    id UUID PRIMARY KEY,
    content TEXT
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
) WITH default_time_to_live = 3600;  -- Data expires after 1 hour
```

## IF NOT EXISTS

Prevent errors if table already exists:

```sql
CREATE TABLE IF NOT EXISTS users (
    user_id UUID PRIMARY KEY,
    username TEXT,
    email TEXT
);
```

## Static Columns

Static columns are shared among all rows in a partition:

```sql
CREATE TABLE user_stats (
    user_id UUID,
    month TEXT,
    total_posts INT STATIC,  -- Same value for all months of a user
    monthly_posts INT,
    PRIMARY KEY (user_id, month)
);
```

## Complete Example

Here's a comprehensive example showing various features:

```sql
CREATE TABLE IF NOT EXISTS user_activities (
    user_id UUID,
    activity_id TIMEUUID,
    activity_type TEXT,
    activity_data MAP<TEXT, TEXT>,
    tags SET<TEXT>,
    created_at TIMESTAMP,
    updated_at TIMESTAMP,
    is_public BOOLEAN,
    view_count COUNTER,
    PRIMARY KEY (user_id, activity_id)
) WITH CLUSTERING ORDER BY (activity_id DESC)
    AND compaction = {'class': 'LeveledCompactionStrategy'}
    AND compression = {'class': 'LZ4Compressor'}
    AND caching = {'keys': 'ALL', 'rows_per_partition': '100'}
    AND comment = 'Stores user activity data'
    AND default_time_to_live = 2592000;  -- 30 days
```

## Best Practices

1. **Choose the right partition key**: Ensure even data distribution
2. **Avoid large partitions**: Keep partition size under 100MB
3. **Use appropriate data types**: Don't use TEXT for everything
4. **Design for your queries**: Create tables based on how you'll query the data
5. **Consider using TTL**: For temporary or time-bound data
6. **Use IF NOT EXISTS**: To make scripts idempotent

## Common Mistakes to Avoid

1. **Hot partitions**: Using timestamps or incremental IDs as partition keys
2. **Too many columns**: Cassandra has a limit of ~2 billion columns per partition
3. **Large collections**: Keep collections small (< 100 elements)
4. **Missing clustering columns**: When you need to store multiple rows per partition

## Related Operations

After creating a table, you might want to:
- Insert data into the table
- Create indexes for additional query patterns
- Alter the table structure
- Create materialized views for different access patterns