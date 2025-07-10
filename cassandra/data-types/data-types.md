# Data Types in Cassandra

Cassandra supports a rich set of data types for storing different kinds of information. Understanding these data types is essential for designing efficient schemas and choosing the right type for your data.

## Native Data Types

### Numeric Types

#### Integer Types

```sql
CREATE TABLE numeric_examples (
    tiny_col    TINYINT,      -- 8-bit signed integer (-128 to 127)
    small_col   SMALLINT,     -- 16-bit signed integer
    regular_col INT,          -- 32-bit signed integer
    big_col     BIGINT,       -- 64-bit signed integer
    var_col     VARINT        -- Arbitrary precision integer
);
```

#### Floating Point Types

```sql
CREATE TABLE float_examples (
    float_col   FLOAT,        -- 32-bit floating point
    double_col  DOUBLE,       -- 64-bit floating point
    decimal_col DECIMAL       -- Variable precision decimal
);
```

### Text Types

```sql
CREATE TABLE text_examples (
    id          UUID PRIMARY KEY,
    ascii_col   ASCII,        -- ASCII character string
    text_col    TEXT,         -- UTF-8 encoded string
    varchar_col VARCHAR       -- UTF-8 encoded string (same as TEXT)
);
```

### Boolean Type

```sql
CREATE TABLE user_settings (
    user_id     INT PRIMARY KEY,
    is_active   BOOLEAN,
    email_verified BOOLEAN
);

-- Insert boolean values
INSERT INTO user_settings (user_id, is_active, email_verified) 
VALUES (1, true, false);
```

### Binary Type

```sql
CREATE TABLE binary_data (
    id          INT PRIMARY KEY,
    file_data   BLOB,         -- Binary large object
    thumbnail   BLOB
);
```

### Date and Time Types

```sql
CREATE TABLE temporal_data (
    id          INT PRIMARY KEY,
    created_date DATE,        -- Date without time
    created_time TIME,        -- Time without date
    created_at  TIMESTAMP,    -- Date and time with millisecond precision
    duration    DURATION      -- Length of time
);

-- Insert examples
INSERT INTO temporal_data (id, created_date, created_time, created_at, duration)
VALUES (1, '2024-01-15', '14:30:00', toTimestamp(now()), 3h30m);
```

### UUID Types

```sql
CREATE TABLE uuid_examples (
    id          UUID PRIMARY KEY,              -- Type 4 UUID
    time_id     TIMEUUID,                     -- Type 1 UUID (time-based)
    username    TEXT
);

-- Insert with UUID functions
INSERT INTO uuid_examples (id, time_id, username)
VALUES (uuid(), now(), 'john_doe');
```

### Internet Address Types

```sql
CREATE TABLE network_data (
    id          INT PRIMARY KEY,
    ip_address  INET,         -- IPv4 or IPv6 address
    server_name TEXT
);

-- Insert IP addresses
INSERT INTO network_data (id, ip_address, server_name)
VALUES (1, '192.168.1.100', 'web-server-1');

INSERT INTO network_data (id, ip_address, server_name)
VALUES (2, '2001:0db8:85a3:0000:0000:8a2e:0370:7334', 'web-server-2');
```

## Collection Types

### List

Ordered collection of elements:

```sql
CREATE TABLE user_activities (
    user_id     INT PRIMARY KEY,
    activities  LIST<TEXT>,
    scores      LIST<INT>
);

-- Insert lists
INSERT INTO user_activities (user_id, activities, scores)
VALUES (1, ['login', 'view_profile', 'logout'], [100, 95, 80]);

-- Update lists
UPDATE user_activities SET activities = activities + ['purchase'] WHERE user_id = 1;
UPDATE user_activities SET activities = ['new_activity'] + activities WHERE user_id = 1;
```

### Set

Unordered collection of unique elements:

```sql
CREATE TABLE user_tags (
    user_id     INT PRIMARY KEY,
    tags        SET<TEXT>,
    skills      SET<TEXT>
);

-- Insert sets
INSERT INTO user_tags (user_id, tags, skills)
VALUES (1, {'developer', 'blogger', 'speaker'}, {'java', 'python', 'cassandra'});

-- Add to set
UPDATE user_tags SET tags = tags + {'writer'} WHERE user_id = 1;

-- Remove from set
UPDATE user_tags SET tags = tags - {'blogger'} WHERE user_id = 1;
```

### Map

Key-value pairs:

```sql
CREATE TABLE user_preferences (
    user_id     INT PRIMARY KEY,
    settings    MAP<TEXT, TEXT>,
    scores      MAP<TEXT, INT>
);

-- Insert maps
INSERT INTO user_preferences (user_id, settings, scores)
VALUES (1, {'theme': 'dark', 'language': 'en'}, {'math': 95, 'science': 87});

-- Update map values
UPDATE user_preferences SET settings['theme'] = 'light' WHERE user_id = 1;
UPDATE user_preferences SET settings = settings + {'timezone': 'UTC'} WHERE user_id = 1;
```

## User-Defined Types (UDT)

Create custom types:

```sql
-- Create a UDT
CREATE TYPE address (
    street TEXT,
    city TEXT,
    state TEXT,
    zip_code TEXT,
    country TEXT
);

-- Use UDT in table
CREATE TABLE users (
    user_id         INT PRIMARY KEY,
    username        TEXT,
    home_address    address,
    work_address    address
);

-- Insert with UDT
INSERT INTO users (user_id, username, home_address, work_address)
VALUES (1, 'john_doe', 
    {street: '123 Main St', city: 'New York', state: 'NY', zip_code: '10001', country: 'USA'},
    {street: '456 Work Ave', city: 'New York', state: 'NY', zip_code: '10002', country: 'USA'}
);
```

## Tuple Type

Fixed-length list of typed elements:

```sql
CREATE TABLE coordinates (
    id          INT PRIMARY KEY,
    location    TUPLE<DOUBLE, DOUBLE>,        -- latitude, longitude
    dimensions  TUPLE<INT, INT, INT>          -- width, height, depth
);

-- Insert tuples
INSERT INTO coordinates (id, location, dimensions)
VALUES (1, (40.7128, -74.0060), (100, 200, 50));
```

## Frozen Collections

Immutable collections treated as single values:

```sql
CREATE TABLE frozen_example (
    id          INT PRIMARY KEY,
    tags        FROZEN<SET<TEXT>>,
    metadata    FROZEN<MAP<TEXT, TEXT>>
);

-- Frozen collections must be replaced entirely
INSERT INTO frozen_example (id, tags, metadata)
VALUES (1, {'tag1', 'tag2'}, {'key1': 'value1', 'key2': 'value2'});

-- Update requires replacing entire frozen collection
UPDATE frozen_example 
SET tags = {'tag1', 'tag2', 'tag3'} 
WHERE id = 1;
```

## Counter Type

Special type for distributed counters:

```sql
CREATE TABLE page_views (
    page_id     TEXT PRIMARY KEY,
    view_count  COUNTER
);

-- Increment counter
UPDATE page_views SET view_count = view_count + 1 WHERE page_id = 'home';
UPDATE page_views SET view_count = view_count + 5 WHERE page_id = 'products';

-- Decrement counter
UPDATE page_views SET view_count = view_count - 1 WHERE page_id = 'home';
```

## Type Conversion Functions

```sql
-- Convert between types
SELECT 
    CAST(123 AS TEXT) AS int_to_text,
    CAST('456' AS INT) AS text_to_int,
    toDate(now()) AS timestamp_to_date,
    toTimestamp(now()) AS timeuuid_to_timestamp,
    toUnixTimestamp(now()) AS to_unix_timestamp
FROM system.local;
```

## Best Practices

1. **Choose appropriate data types**: Use the smallest type that can accommodate your data
2. **Use collections sparingly**: Collections can impact performance if they grow too large
3. **Consider frozen collections**: For better performance when you don't need to update individual elements
4. **Use UDTs for complex structures**: Better than multiple columns for related data
5. **Be careful with counters**: They have special restrictions and can't be mixed with non-counter columns
6. **Understand type limitations**: Some types have size limits (e.g., collections limited to 64KB)

## Common Data Type Mistakes

1. **Using TEXT for everything**: Choose specific types for better validation and storage efficiency
2. **Large collections**: Keep collections small (< 100 elements) for best performance
3. **Mixing counter and non-counter columns**: Not allowed in the same table
4. **Not using appropriate time types**: Use TIMESTAMP for points in time, DURATION for time periods