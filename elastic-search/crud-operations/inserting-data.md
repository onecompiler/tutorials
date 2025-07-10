# Inserting Data in Elasticsearch

Learn how to insert documents into Elasticsearch using various methods and options. This tutorial covers single document insertion, bulk operations, and important concepts for data indexing.

## Prerequisites

Before inserting data, ensure you have:
1. Elasticsearch running (check with `curl localhost:9200`)
2. An index created (or use automatic index creation)

## Basic Document Insertion

### Create Document with ID

Add a document to an index with a specific ID:

```json
PUT /products/_doc/1
{
  "name": "Laptop Pro",
  "category": "Electronics",
  "price": 1299.99,
  "in_stock": true,
  "specs": {
    "cpu": "Intel i7",
    "ram": "16GB",
    "storage": "512GB SSD"
  }
}
```

Response:
```json
{
  "_index": "products",
  "_id": "1",
  "_version": 1,
  "result": "created",
  "_shards": {
    "total": 2,
    "successful": 1,
    "failed": 0
  },
  "_seq_no": 0,
  "_primary_term": 1
}
```

### Understanding the Response

- `_index`: The index where document was stored
- `_id`: The document's unique identifier
- `_version`: Version number (increments with updates)
- `result`: Operation result (created/updated)
- `_shards`: Shard replication information
- `_seq_no`: Sequence number for optimistic concurrency control
- `_primary_term`: Primary term for the primary shard

## Document Creation Methods

### 1. PUT with ID (Create or Update)

```json
PUT /users/_doc/100
{
  "username": "john_doe",
  "email": "john@example.com",
  "registered_date": "2024-01-15"
}
```

This will create a new document or update if ID exists.

### 2. Create Only (Fail if Exists)

Using `op_type` parameter:
```json
PUT /users/_doc/100?op_type=create
{
  "username": "jane_doe",
  "email": "jane@example.com"
}
```

Or using `_create` endpoint:
```json
PUT /users/_create/100
{
  "username": "jane_doe",
  "email": "jane@example.com"
}
```

### 3. Automatic ID Generation

Let Elasticsearch generate a unique ID:

```json
POST /logs/_doc
{
  "timestamp": "2024-01-15T10:30:00",
  "level": "INFO",
  "message": "Application started successfully",
  "service": "auth-service"
}
```

Response includes generated ID:
```json
{
  "_index": "logs",
  "_id": "dXuSt4sBX_Z_kb8rP3qY",  // Auto-generated ID
  "_version": 1,
  "result": "created"
}
```

## Bulk Operations

For inserting multiple documents efficiently:

```json
POST /_bulk
{ "index": { "_index": "products", "_id": "2" } }
{ "name": "Smartphone", "price": 699.99, "category": "Electronics" }
{ "index": { "_index": "products", "_id": "3" } }
{ "name": "Tablet", "price": 499.99, "category": "Electronics" }
{ "index": { "_index": "products" } }
{ "name": "Headphones", "price": 199.99, "category": "Audio" }
```

Note: Each action and document must be on separate lines, ending with a newline.

### Bulk Insert from File

```bash
curl -X POST "localhost:9200/_bulk" \
  -H "Content-Type: application/json" \
  --data-binary @products.json
```

## Advanced Insertion Options

### 1. With Routing

Route documents to specific shards:

```json
PUT /orders/_doc/1001?routing=user123
{
  "order_id": "1001",
  "user_id": "user123",
  "total": 299.99,
  "items": ["item1", "item2"]
}
```

### 2. With Refresh

Make document immediately searchable:

```json
PUT /realtime/_doc/1?refresh=true
{
  "message": "This will be immediately searchable"
}
```

Refresh options:
- `true`: Refresh immediately (impacts performance)
- `wait_for`: Wait for next refresh
- `false`: Don't wait (default)

### 3. With Pipeline

Apply ingest pipeline during indexing:

```json
PUT /logs/_doc/1?pipeline=add-timestamp
{
  "message": "Log entry",
  "level": "INFO"
}
```

### 4. With Timeout

Set operation timeout:

```json
PUT /products/_doc/1?timeout=5m
{
  "name": "Special Product",
  "processing_required": true
}
```

## Working with Different Data Types

### Nested Objects

```json
POST /employees/_doc
{
  "name": "Alice Johnson",
  "department": "Engineering",
  "contact": {
    "email": "alice@company.com",
    "phone": "+1-555-0123"
  },
  "projects": [
    {
      "name": "Project A",
      "status": "active"
    },
    {
      "name": "Project B",
      "status": "completed"
    }
  ]
}
```

### Arrays

```json
POST /articles/_doc
{
  "title": "Elasticsearch Tutorial",
  "tags": ["elasticsearch", "search", "database"],
  "authors": [
    "John Smith",
    "Jane Doe"
  ],
  "ratings": [4.5, 4.8, 4.2]
}
```

### Date Formats

```json
POST /events/_doc
{
  "event_name": "Conference",
  "start_date": "2024-03-15",
  "timestamp": "2024-03-15T09:00:00Z",
  "epoch_millis": 1710489600000
}
```

## Index Templates and Dynamic Mapping

### Create with Dynamic Fields

```json
POST /dynamic_index/_doc
{
  "static_field": "known value",
  "dynamic_string": "Elasticsearch will detect this as text",
  "dynamic_number": 42,
  "dynamic_boolean": true,
  "dynamic_date": "2024-01-15"
}
```

### Explicit Mapping Before Insert

```json
PUT /products
{
  "mappings": {
    "properties": {
      "name": { "type": "text" },
      "price": { "type": "float" },
      "in_stock": { "type": "boolean" },
      "created_at": { "type": "date" }
    }
  }
}
```

## Best Practices

### 1. Use Bulk API for Multiple Documents

Instead of:
```json
PUT /index/_doc/1 {...}
PUT /index/_doc/2 {...}
PUT /index/_doc/3 {...}
```

Use:
```json
POST /_bulk
{"index": {"_index": "index", "_id": "1"}}
{...}
{"index": {"_index": "index", "_id": "2"}}
{...}
```

### 2. Choose Appropriate ID Strategy

- **User-provided IDs**: When you need predictable, meaningful IDs
- **Auto-generated IDs**: For logs, events, or when ID doesn't matter

### 3. Consider Document Size

- Keep documents under 100MB (hard limit)
- Optimal size: 1KB - 100KB
- Split large documents into smaller ones

### 4. Handle Versioning

```json
PUT /products/_doc/1?version=2&version_type=external
{
  "name": "Updated Product",
  "version": 2
}
```

## Common Errors and Solutions

### 1. Index Not Found

```json
{
  "error": {
    "type": "index_not_found_exception",
    "reason": "no such index [products]"
  }
}
```

Solution: Create index first or enable auto-create:
```json
PUT /products
```

### 2. Document Already Exists

When using `_create`:
```json
{
  "error": {
    "type": "version_conflict_engine_exception",
    "reason": "[1]: version conflict, document already exists"
  }
}
```

Solution: Use PUT without `_create` or update the document.

### 3. Mapping Conflict

```json
{
  "error": {
    "type": "mapper_parsing_exception",
    "reason": "failed to parse field [price] of type [long]"
  }
}
```

Solution: Ensure data types match the mapping.

## Performance Tips

1. **Bulk Size**: Keep bulk requests between 5-15 MB
2. **Refresh Interval**: Increase for better indexing performance
3. **Replicas**: Set to 0 during initial bulk load, then increase
4. **Sharding**: Plan shard count based on data volume

## Next Steps

After mastering data insertion:
1. Learn about reading and searching data
2. Understand update operations
3. Explore bulk processing patterns
4. Study index optimization techniques