# Creating a Keyspace

A keyspace in Cassandra is the top-level container for tables, similar to a database in relational database systems. It defines important properties including replication strategy and replication factor that determine how data is distributed and replicated across the cluster.

## Syntax

```sql
CREATE KEYSPACE [IF NOT EXISTS] keyspace_name
WITH REPLICATION = {
    'class' : 'replication_strategy',
    'replication_factor' : N
}
[AND DURABLE_WRITES = true|false];
```

## Replication Strategies

### SimpleStrategy

Used for single data center deployments or development environments. Not recommended for production as it doesn't consider rack or data center placement.

```sql
CREATE KEYSPACE dev_keyspace
WITH REPLICATION = {
    'class' : 'SimpleStrategy',
    'replication_factor' : 3
};
```

### NetworkTopologyStrategy

Recommended for production and multi-data center deployments. Allows you to specify replication factor per data center.

```sql
CREATE KEYSPACE prod_keyspace
WITH REPLICATION = {
    'class' : 'NetworkTopologyStrategy',
    'datacenter1' : 3,
    'datacenter2' : 2
};
```

## Replication Factor

The replication factor determines how many copies of your data are maintained:
- **RF = 1**: Only one copy (no fault tolerance)
- **RF = 3**: Three copies (recommended minimum for production)
- **RF = 5**: Five copies (for critical data requiring high availability)

For production environments, use at least RF = 3 to ensure no single point of failure.

## Examples

### Creating a Simple Keyspace

```sql
CREATE KEYSPACE tutorial_db
WITH REPLICATION = {
    'class' : 'SimpleStrategy',
    'replication_factor' : 3
}
AND DURABLE_WRITES = true;
```

### Creating a Multi-Data Center Keyspace

```sql
CREATE KEYSPACE global_app
WITH REPLICATION = {
    'class' : 'NetworkTopologyStrategy',
    'us_east' : 3,
    'us_west' : 2,
    'europe' : 3
};
```

### Creating Keyspace If Not Exists

```sql
CREATE KEYSPACE IF NOT EXISTS my_app
WITH REPLICATION = {
    'class' : 'SimpleStrategy',
    'replication_factor' : 1
};
```


## Viewing Keyspaces

### List All Keyspaces

```sql
DESCRIBE KEYSPACES;
```

Output example:
```
system_schema  system_auth  system  system_distributed  system_traces  tutorial_db
```

### View Keyspace Details

```sql
DESCRIBE KEYSPACE tutorial_db;
```

This shows the complete keyspace definition including all tables and their schemas.

## Using a Keyspace

To switch to a keyspace and execute commands within it:

```sql
USE tutorial_db;
```

After this command, all subsequent table operations will be performed within the selected keyspace.

## Additional Options

### Durable Writes

The `DURABLE_WRITES` option determines whether Cassandra writes to the commit log for this keyspace:
- `true` (default): Writes go to commit log for durability
- `false`: Bypass commit log (use only for data you can afford to lose)

```sql
CREATE KEYSPACE temp_data
WITH REPLICATION = {
    'class' : 'SimpleStrategy',
    'replication_factor' : 1
}
AND DURABLE_WRITES = false;
```

## Best Practices

1. **Always use NetworkTopologyStrategy for production**
2. **Set replication factor to at least 3 for production data**
3. **Consider your consistency requirements when setting replication**
4. **Use meaningful keyspace names that reflect the application or domain**
5. **Plan your replication strategy based on your data center topology**