# Cassandra Architecture

Understanding Cassandra's architecture is crucial for designing efficient applications and troubleshooting issues. This tutorial covers the key architectural components and concepts.

## Overview

Cassandra is a distributed database designed to handle large amounts of data across many servers with no single point of failure. Its architecture is based on two key technologies:
- **Amazon Dynamo**: Distributed systems techniques
- **Google BigTable**: Data model and storage engine

## Key Architectural Principles

### Peer-to-Peer Architecture

Unlike master-slave architectures, Cassandra uses a peer-to-peer distributed system where:
- All nodes are equal (no master/slave)
- Any node can handle any request
- Nodes communicate via gossip protocol
- No single point of failure

### Data Distribution

#### Consistent Hashing

Cassandra uses consistent hashing to distribute data:

```sql
-- The partition key determines which node stores the data
CREATE TABLE users (
    user_id UUID PRIMARY KEY,  -- Partition key
    username TEXT,
    email TEXT
);

-- Data is distributed based on hash of partition key
-- hash(user_id) -> token -> node
```

#### Token Ring

- Each node is assigned token ranges
- Data is placed on nodes based on partition key hash
- Virtual nodes (vnodes) improve distribution

### Replication

#### Replication Strategy

```sql
-- SimpleStrategy for single datacenter
CREATE KEYSPACE single_dc
WITH REPLICATION = {
    'class': 'SimpleStrategy',
    'replication_factor': 3
};

-- NetworkTopologyStrategy for multiple datacenters
CREATE KEYSPACE multi_dc
WITH REPLICATION = {
    'class': 'NetworkTopologyStrategy',
    'dc1': 3,
    'dc2': 2
};
```

#### Replication Process

1. Coordinator node receives write request
2. Calculates which nodes should store replicas
3. Sends write to all replica nodes in parallel
4. Waits for acknowledgments based on consistency level

## Core Components

### Node

A single Cassandra instance running on a server:

```
Node Components:
├── Commit Log (durability)
├── Memtables (in-memory data)
├── SSTables (on-disk data)
├── Cache (row and key cache)
├── Compaction (SSTable maintenance)
└── Gossip (cluster communication)
```

### Cluster

Collection of nodes that contain your data:

```
Cluster
├── Datacenter 1
│   ├── Rack 1
│   │   ├── Node 1
│   │   └── Node 2
│   └── Rack 2
│       ├── Node 3
│       └── Node 4
└── Datacenter 2
    └── ...
```

### Datacenter

Logical grouping of nodes, typically by:
- Geographic location
- Cloud availability zone
- Workload separation

### Rack

Logical grouping within a datacenter:
- Represents failure domain
- Cassandra ensures replicas span racks
- Can map to physical racks or cloud AZs

## Write Path

### Write Process

1. **Commit Log**: Write is first recorded for durability
2. **Memtable**: Data written to in-memory structure
3. **Acknowledgment**: Based on consistency level
4. **Flush**: Memtable flushed to SSTable when full

```
Client Write Request
    ↓
Coordinator Node
    ↓
Commit Log (durability)
    ↓
Memtable (memory)
    ↓
[Eventually]
    ↓
SSTable (disk)
```

### Consistency Levels for Writes

```sql
-- Write with consistency level
INSERT INTO users (user_id, username) 
VALUES (uuid(), 'john_doe')
USING CONSISTENCY QUORUM;
```

Common write consistency levels:
- **ANY**: At least one node (including hinted handoff)
- **ONE**: At least one replica node
- **QUORUM**: Majority of replicas (RF/2 + 1)
- **ALL**: All replica nodes

## Read Path

### Read Process

1. **Coordinator**: Receives read request
2. **Replica Selection**: Chooses fastest replicas
3. **Data Retrieval**: 
   - Check row cache
   - Check memtables
   - Check SSTables (with bloom filters)
4. **Read Repair**: Fix inconsistencies
5. **Return Result**: To client

```
Client Read Request
    ↓
Coordinator Node
    ↓
[Check Multiple Sources]
├── Row Cache (if enabled)
├── Memtables
└── SSTables
    ├── Bloom Filter (probably has data?)
    ├── Key Cache (SSTable offset)
    ├── Partition Summary
    ├── Partition Index
    └── Data File
```

### Read Consistency Levels

```sql
-- Read with consistency level
SELECT * FROM users 
WHERE user_id = 123e4567-e89b-12d3-a456-426614174000
USING CONSISTENCY LOCAL_QUORUM;
```

## Storage Engine

### SSTable (Sorted String Table)

Immutable on-disk files containing data:

```
SSTable Components:
├── Data.db (actual data)
├── Index.db (index of partitions)
├── Filter.db (bloom filter)
├── Summary.db (partition summary)
├── Statistics.db (metadata)
└── CompressionInfo.db (compression metadata)
```

### Compaction

Process of merging SSTables:

```sql
-- Compaction strategies
ALTER TABLE users 
WITH compaction = {
    'class': 'SizeTieredCompactionStrategy',
    'min_threshold': 4,
    'max_threshold': 32
};

-- Other strategies:
-- LeveledCompactionStrategy
-- TimeWindowCompactionStrategy
-- DateTieredCompactionStrategy (deprecated)
```

### Memtables

In-memory structures for recent writes:
- One memtable per table
- Flushed to SSTable when full
- Configurable size thresholds

## Consistency and Availability

### CAP Theorem

Cassandra is an AP system:
- **Available**: System remains operational
- **Partition Tolerant**: Handles network splits
- **Eventually Consistent**: Not immediately consistent

### Tunable Consistency

Balance between consistency and availability:

```sql
-- Strong consistency (R + W > RF)
-- RF=3, W=QUORUM(2), R=QUORUM(2)
-- 2 + 2 > 3 ✓

-- Eventual consistency
-- RF=3, W=ONE, R=ONE
-- 1 + 1 < 3 ✗
```

### Hinted Handoff

Temporary storage for unavailable nodes:
- Coordinator stores hints for down nodes
- Replays writes when node returns
- Configurable hint window (default 3 hours)

## Gossip Protocol

Peer-to-peer communication protocol:

```
Gossip Information:
├── Node State (UP/DOWN)
├── Load Information
├── Schema Version
├── Token Ownership
└── Datacenter/Rack Info
```

Gossip characteristics:
- Runs every second
- Exchanges state with up to 3 nodes
- Efficient epidemic protocol
- Enables automatic discovery

## Coordinator Node

Any node can be a coordinator:

```
Coordinator Responsibilities:
├── Route requests to replicas
├── Handle consistency requirements
├── Manage read repair
├── Store hinted handoffs
└── Return results to client
```

## Partitioner

Determines data distribution:

```sql
-- Default: Murmur3Partitioner (recommended)
-- hash(partition_key) → token → node

-- Example token ranges
-- Node A: -9223372036854775808 to -4611686018427387904
-- Node B: -4611686018427387903 to 0
-- Node C: 1 to 4611686018427387903
-- Node D: 4611686018427387904 to 9223372036854775807
```

## Caching

### Row Cache

Caches entire rows:
```sql
ALTER TABLE users WITH caching = {
    'keys': 'ALL',
    'rows_per_partition': '100'
};
```

### Key Cache

Caches partition key locations in SSTables:
- Enabled by default
- Reduces disk seeks
- Configurable size

## Best Practices

1. **Design for your queries**: Model data based on access patterns
2. **Use appropriate consistency levels**: Balance consistency vs performance
3. **Monitor cluster health**: Track metrics and performance
4. **Plan capacity**: Account for replication and growth
5. **Configure properly**: Tune based on workload
6. **Understand failure modes**: Plan for node/rack/DC failures

## Common Architecture Patterns

### Multi-Datacenter Deployment

```sql
CREATE KEYSPACE global_app
WITH REPLICATION = {
    'class': 'NetworkTopologyStrategy',
    'us_east': 3,
    'us_west': 3,
    'eu_west': 3
};

-- Use LOCAL_QUORUM for datacenter-aware consistency
SELECT * FROM users USING CONSISTENCY LOCAL_QUORUM;
```

### Workload Separation

- Use separate datacenters for:
  - Analytics workloads
  - Real-time serving
  - Backup/disaster recovery

### Geographic Distribution

- Place datacenters near users
- Use consistency levels that respect locality
- Consider network latency in design