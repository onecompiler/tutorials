In simpler terms, we can say INDEX is a pointer to some data in a table. INDEXES are very useful in attaining higher performance especially for large databases as it speeds up your search query execution.

Unindexed tables take more time when compared to Indexed tables. For example, you can consider the synopsis of a book as an example for Indexes. You can easily search for a particular topic by referring it's page number in synopsis.

## 1. CREATE INDEX

CREATE INDEX is used to create an index. INDEXES can be created on multiple columns (if required) of a table. Search query corresponding to INDEX column will be much faster than on other columns.

### Syntax:
```sql
CREATE INDEX index_name ON table_name(column_name);

-- With Oracle-specific options
CREATE INDEX index_name ON table_name(column_name)
  TABLESPACE tablespace_name
  STORAGE (INITIAL 10K NEXT 10K)
  PCTFREE 10
  PARALLEL 4
  NOLOGGING;

-- Composite index
CREATE INDEX index_name ON table_name(column1, column2, column3);
```

## 2. CREATE UNIQUE INDEX

A unique index ensures that no two rows have the same values in the indexed columns.

### Syntax:
```sql
CREATE UNIQUE INDEX index_name ON table_name(column_name);

-- With options
CREATE UNIQUE INDEX index_name ON table_name(column_name)
  TABLESPACE index_ts
  COMPRESS;
```

## 3. Function-Based Indexes

Oracle allows creating indexes on expressions or functions applied to columns.

### Syntax:
```sql
-- Simple function-based index
CREATE INDEX index_name ON table_name(UPPER(column_name));

-- Complex expression
CREATE INDEX index_name ON table_name(SUBSTR(column_name, 1, 10));

-- Multiple expressions
CREATE INDEX index_name ON table_name(UPPER(last_name), LOWER(first_name));
```

## 4. Bitmap Indexes

Bitmap indexes are efficient for columns with low cardinality (few distinct values).

### Syntax:
```sql
CREATE BITMAP INDEX index_name ON table_name(column_name);

-- With options
CREATE BITMAP INDEX index_name ON table_name(column_name)
  TABLESPACE bitmap_ts
  PARALLEL 4;
```

## 5. DROP INDEX

DROP INDEX is used to delete an existing index. In Oracle, you don't specify the table name.

### Syntax:
```sql
DROP INDEX index_name;

-- Force drop even if constraints depend on it
DROP INDEX index_name FORCE;
```

## 6. REBUILD INDEX

Rebuilding an index can improve performance and reclaim space.

### Syntax:
```sql
-- Basic rebuild
ALTER INDEX index_name REBUILD;

-- Rebuild with options
ALTER INDEX index_name REBUILD
  TABLESPACE new_tablespace
  ONLINE
  PARALLEL 4
  NOLOGGING;

-- Rebuild partition
ALTER INDEX index_name REBUILD PARTITION partition_name;
```

## 7. Monitoring Indexes

Oracle provides ways to monitor index usage and performance.

### Syntax:
```sql
-- Enable index monitoring
ALTER INDEX index_name MONITORING USAGE;

-- Disable monitoring
ALTER INDEX index_name NOMONITORING USAGE;

-- Check if index is being used
SELECT * FROM V$OBJECT_USAGE WHERE INDEX_NAME = 'index_name';

-- Analyze index
ANALYZE INDEX index_name VALIDATE STRUCTURE;

-- Get index statistics
ANALYZE INDEX index_name COMPUTE STATISTICS;
```

## 8. Oracle-Specific Index Types

### Reverse Key Index
Useful for reducing index block contention in RAC environments.
```sql
CREATE INDEX index_name ON table_name(column_name) REVERSE;
```

### Invisible Index
Indexes that are maintained but ignored by the optimizer unless explicitly specified.
```sql
CREATE INDEX index_name ON table_name(column_name) INVISIBLE;

-- Make visible
ALTER INDEX index_name VISIBLE;
```

### Compressed Index
Reduces storage requirements for indexes.
```sql
CREATE INDEX index_name ON table_name(column1, column2) COMPRESS 1;
```

### Domain Index
Used for complex data types like spatial or text data.
```sql
CREATE INDEX index_name ON table_name(column_name)
  INDEXTYPE IS MDSYS.SPATIAL_INDEX;
```

### Local and Global Partitioned Indexes
For partitioned tables.
```sql
-- Local partitioned index
CREATE INDEX index_name ON table_name(column_name) LOCAL;

-- Global partitioned index
CREATE INDEX index_name ON table_name(column_name)
  GLOBAL PARTITION BY RANGE (column_name)
  (PARTITION p1 VALUES LESS THAN (100),
   PARTITION p2 VALUES LESS THAN (200));
```

## 9. Index Hints

Force Oracle to use or ignore specific indexes.
```sql
-- Use index hint
SELECT /*+ INDEX(table_name index_name) */ * 
FROM table_name 
WHERE column_name = value;

-- Ignore index hint
SELECT /*+ NO_INDEX(table_name index_name) */ * 
FROM table_name 
WHERE column_name = value;
```

## 10. Index Management Views

Oracle provides several views to manage indexes:
```sql
-- All indexes for current user
SELECT * FROM USER_INDEXES;

-- Index columns
SELECT * FROM USER_IND_COLUMNS WHERE INDEX_NAME = 'index_name';

-- Index statistics
SELECT * FROM USER_IND_STATISTICS WHERE INDEX_NAME = 'index_name';

-- Index partitions
SELECT * FROM USER_IND_PARTITIONS WHERE INDEX_NAME = 'index_name';
```