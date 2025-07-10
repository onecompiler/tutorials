# Consistency Levels

Consistency levels in Cassandra control how many replicas must acknowledge a read or write operation before it's considered successful. This tutorial explains how to use consistency levels to balance between consistency, availability, and performance.

## Overview

Cassandra offers tunable consistency, allowing you to choose the appropriate level for each operation based on your requirements.

## Write Consistency Levels

### ANY

Lowest consistency level, highest availability:

```sql
INSERT INTO users (user_id, username) 
VALUES (uuid(), 'john_doe')
USING CONSISTENCY ANY;
```

- Write succeeds if any node receives it (including hinted handoff)
- Lowest latency
- Risk of data loss if hint is lost

### ONE, TWO, THREE

Fixed number of replicas:

```sql
-- At least one replica
INSERT INTO users (user_id, username) 
VALUES (uuid(), 'jane_doe')
USING CONSISTENCY ONE;

-- Exactly two replicas
UPDATE users SET email = 'jane@example.com'
WHERE user_id = 123e4567-e89b-12d3-a456-426614174000
USING CONSISTENCY TWO;

-- Exactly three replicas
DELETE FROM users
WHERE user_id = 123e4567-e89b-12d3-a456-426614174000
USING CONSISTENCY THREE;
```

### QUORUM

Majority of replicas:

```sql
INSERT INTO important_data (id, value) 
VALUES (uuid(), 'critical info')
USING CONSISTENCY QUORUM;
```

- Formula: (RF / 2) + 1
- RF=3: Requires 2 replicas
- RF=5: Requires 3 replicas
- Good balance of consistency and availability

### LOCAL_QUORUM

Quorum within local datacenter:

```sql
INSERT INTO users (user_id, username) 
VALUES (uuid(), 'local_user')
USING CONSISTENCY LOCAL_QUORUM;
```

- Avoids cross-datacenter latency
- Commonly used in multi-DC deployments
- Maintains consistency within DC

### EACH_QUORUM

Quorum in each datacenter:

```sql
INSERT INTO global_config (key, value) 
VALUES ('setting1', 'value1')
USING CONSISTENCY EACH_QUORUM;
```

- Requires quorum in every DC
- Highest consistency across DCs
- Lower availability

### ALL

All replicas must acknowledge:

```sql
INSERT INTO critical_data (id, value) 
VALUES (uuid(), 'must not lose')
USING CONSISTENCY ALL;
```

- Highest consistency
- Lowest availability
- Any node failure causes operation failure

## Read Consistency Levels

### ONE

Read from one replica:

```sql
SELECT * FROM users 
WHERE user_id = 123e4567-e89b-12d3-a456-426614174000
USING CONSISTENCY ONE;
```

- Fastest reads
- May return stale data
- Good for non-critical data

### TWO, THREE

Read from fixed number of replicas:

```sql
SELECT * FROM accounts 
WHERE account_id = 123
USING CONSISTENCY TWO;
```

- Returns most recent data from replicas queried
- Higher latency than ONE

### QUORUM

Read from majority of replicas:

```sql
SELECT balance FROM accounts 
WHERE account_id = 123
USING CONSISTENCY QUORUM;
```

- Ensures latest data when paired with QUORUM writes
- Good consistency guarantee

### LOCAL_QUORUM

Quorum from local datacenter:

```sql
SELECT * FROM users 
WHERE user_id = 123e4567-e89b-12d3-a456-426614174000
USING CONSISTENCY LOCAL_QUORUM;
```

- Avoids cross-DC latency
- Standard for multi-DC reads

### EACH_QUORUM

Not supported for reads, only writes.

### ALL

Read from all replicas:

```sql
SELECT * FROM critical_settings 
WHERE key = 'important_config'
USING CONSISTENCY ALL;
```

- Highest consistency
- Highest latency
- Fails if any replica is down

### SERIAL

Lightweight transaction read:

```sql
SELECT * FROM accounts 
WHERE account_id = 123
USING CONSISTENCY SERIAL;
```

- Used with lightweight transactions
- Ensures linearizable consistency
- Uses Paxos consensus

### LOCAL_SERIAL

Serial consistency within datacenter:

```sql
SELECT * FROM inventory 
WHERE product_id = 456
USING CONSISTENCY LOCAL_SERIAL;
```

- Serial consistency without cross-DC latency
- Used for local lightweight transactions

## Achieving Strong Consistency

### Formula

Strong consistency when: **R + W > RF**

Where:
- R = Read consistency level (number of replicas)
- W = Write consistency level (number of replicas)
- RF = Replication factor

### Examples

```sql
-- RF = 3, W = QUORUM (2), R = QUORUM (2)
-- 2 + 2 > 3 ✓ Strong consistency

-- RF = 3, W = ONE (1), R = ONE (1)
-- 1 + 1 < 3 ✗ Eventual consistency

-- RF = 5, W = THREE (3), R = THREE (3)
-- 3 + 3 > 5 ✓ Strong consistency
```

## Consistency Level Selection

### Use Cases by Consistency Level

#### ANY (Write only)
```sql
-- Logging, metrics, non-critical data
INSERT INTO app_logs (id, timestamp, message) 
VALUES (uuid(), toTimestamp(now()), 'App started')
USING CONSISTENCY ANY;
```

#### ONE
```sql
-- Caching, session data, activity tracking
SELECT cached_data FROM cache 
WHERE key = 'user:123:profile'
USING CONSISTENCY ONE;
```

#### LOCAL_QUORUM
```sql
-- User data, standard application data
UPDATE users SET last_login = toTimestamp(now())
WHERE user_id = 123e4567-e89b-12d3-a456-426614174000
USING CONSISTENCY LOCAL_QUORUM;
```

#### QUORUM
```sql
-- Financial data, inventory, critical business data
UPDATE account_balances SET balance = balance - 100
WHERE account_id = 123
USING CONSISTENCY QUORUM;
```

#### ALL
```sql
-- Configuration, critical settings
INSERT INTO system_config (key, value) 
VALUES ('maintenance_mode', 'true')
USING CONSISTENCY ALL;
```

## Lightweight Transactions

### Compare and Set

```sql
-- Only update if condition is met
UPDATE users 
SET email = 'new@example.com'
WHERE user_id = 123e4567-e89b-12d3-a456-426614174000
IF email = 'old@example.com';
```

### Insert If Not Exists

```sql
INSERT INTO users (user_id, username) 
VALUES (uuid(), 'unique_username')
IF NOT EXISTS;
```

### Serial Consistency Usage

```sql
-- Use SERIAL for strong consistency in LWT
BEGIN BATCH
    UPDATE inventory SET quantity = quantity - 1 
    WHERE product_id = 123 
    IF quantity > 0
APPLY BATCH USING CONSISTENCY SERIAL;
```

## Batch Operations

### Logged Batches

```sql
BEGIN BATCH
    INSERT INTO users (user_id, username) VALUES (uuid(), 'user1');
    INSERT INTO audit_log (id, action) VALUES (uuid(), 'user_created');
APPLY BATCH USING CONSISTENCY QUORUM;
```

### Unlogged Batches

```sql
BEGIN UNLOGGED BATCH
    UPDATE counters SET count = count + 1 WHERE id = 1;
    UPDATE counters SET count = count + 1 WHERE id = 2;
APPLY BATCH USING CONSISTENCY ONE;
```

## Performance Impact

### Latency by Consistency Level

1. **ANY** (writes only): Lowest latency
2. **ONE**: Very low latency
3. **TWO/THREE**: Moderate latency
4. **LOCAL_QUORUM**: Moderate latency
5. **QUORUM**: Higher latency (may include cross-DC)
6. **EACH_QUORUM**: High latency (all DCs)
7. **ALL**: Highest latency

### Availability Impact

Higher consistency = Lower availability:

```
ANY > ONE > TWO > THREE > LOCAL_QUORUM > QUORUM > EACH_QUORUM > ALL
```

## Best Practices

### 1. Default Consistency Levels

```sql
-- Set default for session
CONSISTENCY LOCAL_QUORUM;

-- Override for specific queries
SELECT * FROM less_critical_data USING CONSISTENCY ONE;
```

### 2. Multi-Datacenter Deployments

```sql
-- Prefer LOCAL_QUORUM for most operations
INSERT INTO users (user_id, data) 
VALUES (uuid(), 'user data')
USING CONSISTENCY LOCAL_QUORUM;

-- Use EACH_QUORUM only when necessary
INSERT INTO global_settings (key, value) 
VALUES ('critical_setting', 'value')
USING CONSISTENCY EACH_QUORUM;
```

### 3. Read Repair

```sql
-- Configure read repair chance
ALTER TABLE users 
WITH read_repair_chance = 0.1
AND dclocal_read_repair_chance = 0.1;
```

### 4. Handling Failures

```python
# Example: Retry with lower consistency
try:
    session.execute(query, consistency_level=ConsistencyLevel.QUORUM)
except Exception:
    # Retry with lower consistency
    session.execute(query, consistency_level=ConsistencyLevel.ONE)
```

## Common Patterns

### Write Heavy Workload

```sql
-- Fast writes, eventual consistency
INSERT INTO events (id, data) VALUES (uuid(), 'event')
USING CONSISTENCY ONE;
```

### Read Heavy Workload

```sql
-- Ensure consistency for reads
SELECT * FROM products WHERE id = 123
USING CONSISTENCY LOCAL_QUORUM;
```

### Critical Data

```sql
-- Both read and write at QUORUM
-- Write
INSERT INTO financial_transactions (id, amount) 
VALUES (uuid(), 1000.00)
USING CONSISTENCY QUORUM;

-- Read
SELECT * FROM financial_transactions WHERE id = 123
USING CONSISTENCY QUORUM;
```

## Monitoring and Troubleshooting

### Check Consistency Level

```sql
-- Show current consistency level
CONSISTENCY;
```

### Monitor Failed Operations

Use nodetool to monitor:
```bash
nodetool proxyhistograms
```

### Common Issues

1. **WriteTimeout**: Insufficient replicas responded
2. **ReadTimeout**: Read didn't complete in time
3. **UnavailableException**: Not enough replicas available

## Summary

Choose consistency levels based on:
1. **Data criticality**: Higher consistency for critical data
2. **Performance requirements**: Lower consistency for better performance
3. **Availability needs**: Lower consistency for higher availability
4. **Datacenter topology**: Use LOCAL_* variants for multi-DC
5. **Use case**: Match consistency to business requirements