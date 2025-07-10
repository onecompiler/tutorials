# Key Concepts in Elasticsearch

Understanding these fundamental concepts is essential for working effectively with Elasticsearch. Let's explore each concept with examples to make them clear for beginners.

## Cluster

A cluster is a collection of one or more nodes (servers) that work together to store your data and provide search capabilities across all nodes.

### Key Points:
- Each cluster has a unique name (default: "elasticsearch")
- Nodes join a cluster by using the cluster name
- A cluster can have just one node (single-node cluster)
- Provides automatic load balancing and failover

### Example:
```json
// Check cluster health
GET /_cluster/health

// Response
{
  "cluster_name": "my-application",
  "status": "green",
  "number_of_nodes": 3,
  "number_of_data_nodes": 3
}
```

## Node

A node is a single server that is part of your cluster, stores data, and participates in the cluster's indexing and search capabilities.

### Types of Nodes:
1. **Master Node**: Manages cluster-wide operations
2. **Data Node**: Stores data and executes data-related operations
3. **Ingest Node**: Preprocesses documents before indexing
4. **Coordinating Node**: Routes requests and aggregates results

### Example:
```json
// Check node information
GET /_nodes

// Each node has:
{
  "name": "node-1",
  "transport_address": "127.0.0.1:9300",
  "host": "127.0.0.1",
  "roles": ["master", "data", "ingest"]
}
```

## Index

An index is a collection of documents that have similar characteristics. It's similar to a database in the relational world.

### Key Points:
- Index names must be lowercase
- An index can contain multiple document types (deprecated in newer versions)
- Each index has its own settings and mappings

### Example:
```json
// Create an index
PUT /products
{
  "settings": {
    "number_of_shards": 3,
    "number_of_replicas": 1
  }
}

// Index structure example:
// products (index)
//   ├── laptop (document)
//   ├── phone (document)
//   └── tablet (document)
```

## Document

A document is a basic unit of information that can be indexed. It's expressed in JSON format and stored within an index.

### Key Points:
- Similar to a row in a relational database
- Each document has a unique ID
- Documents are immutable (updates create new versions)
- Can contain nested objects and arrays

### Example:
```json
// A document in the products index
{
  "_index": "products",
  "_id": "1",
  "_source": {
    "name": "iPhone 13",
    "category": "smartphones",
    "price": 999,
    "features": ["5G", "A15 Bionic", "Dual Camera"],
    "manufacturer": {
      "name": "Apple",
      "country": "USA"
    }
  }
}
```

## Shard

A shard is a single Lucene instance and a fundamental unit of storage in Elasticsearch. Each index is divided into shards for scalability.

### Types of Shards:
1. **Primary Shards**: Original shards that hold the data
2. **Replica Shards**: Copies of primary shards for redundancy

### Key Points:
- Number of primary shards is fixed at index creation
- Each shard is a fully functional index
- Shards allow horizontal scaling
- Default: 1 primary shard per index

### Example:
```json
// Creating an index with custom shard settings
PUT /logs
{
  "settings": {
    "number_of_shards": 5,      // 5 primary shards
    "number_of_replicas": 1      // 1 replica per primary shard
  }
}

// Total shards = 5 primary + 5 replica = 10 shards
```

## Replicas

Replicas are copies of primary shards that provide redundancy and improve search performance.

### Benefits:
1. **High Availability**: If a node fails, replicas ensure no data loss
2. **Increased Performance**: Search queries can be executed on replicas
3. **Load Balancing**: Distributes query load across multiple copies

### Example:
```json
// Update replica settings
PUT /products/_settings
{
  "number_of_replicas": 2
}

// With 3 primary shards and 2 replicas:
// Total shards = 3 primary + (3 × 2) replicas = 9 shards
```

## Near Real-Time (NRT)

Elasticsearch is a near real-time search platform, meaning there's a slight delay between indexing a document and when it becomes searchable.

### Key Points:
- Default refresh interval: 1 second
- Documents are searchable within ~1 second of indexing
- Can be configured per index
- Trade-off between real-time and performance

### Example:
```json
// Index a document
POST /products/_doc
{
  "name": "New Product",
  "price": 299
}

// Document is searchable after ~1 second

// Force immediate refresh (not recommended for production)
POST /products/_refresh

// Configure refresh interval
PUT /products/_settings
{
  "index": {
    "refresh_interval": "30s"  // Refresh every 30 seconds
  }
}
```

## Additional Important Concepts

### Mapping

Defines how documents and their fields are stored and indexed.

```json
PUT /products/_mapping
{
  "properties": {
    "name": { "type": "text" },
    "price": { "type": "float" },
    "in_stock": { "type": "boolean" },
    "created_at": { "type": "date" }
  }
}
```

### Inverted Index

The core data structure that makes searching fast:
- Maps terms to the documents containing them
- Similar to an index in a book
- Enables full-text search capabilities

### Example:
```
Term "apple" appears in:
  - Document 1
  - Document 5
  - Document 12

Term "phone" appears in:
  - Document 2
  - Document 5
  - Document 8
```

## How These Concepts Work Together

1. **Data Storage Flow**:
   ```
   Document → Index → Shard → Node → Cluster
   ```

2. **Search Flow**:
   ```
   Query → Cluster → Nodes → Shards → Documents → Results
   ```

3. **Redundancy**:
   ```
   Primary Shard → Replica Shards → Different Nodes
   ```

## Best Practices

1. **Cluster Planning**:
   - Use odd number of master-eligible nodes (3, 5, 7)
   - Separate master and data nodes for large clusters

2. **Index Design**:
   - Keep indices focused on specific data types
   - Use meaningful, lowercase names
   - Plan shard count based on data volume

3. **Shard Sizing**:
   - Aim for 20-40GB per shard
   - Avoid too many small shards
   - Consider future growth

4. **Replica Strategy**:
   - At least 1 replica for production
   - More replicas for read-heavy workloads
   - Ensure enough nodes to distribute replicas

## Common Beginner Mistakes

1. **Too Many Shards**: Creating hundreds of small shards impacts performance
2. **No Replicas**: Running without replicas risks data loss
3. **Wrong Refresh Settings**: Setting refresh to 0 for real-time at the cost of performance
4. **Ignoring Cluster Health**: Not monitoring yellow or red cluster states

## Next Steps

Now that you understand the key concepts, let's proceed to install Elasticsearch and start working with these concepts hands-on.