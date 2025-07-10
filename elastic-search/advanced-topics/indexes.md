# Index Management in Elasticsearch

Learn how to create, configure, and manage indices in Elasticsearch. This guide covers index settings, mappings, aliases, and best practices for index management.

## Understanding Indices

An index in Elasticsearch is similar to a database in traditional systems. It's a collection of documents that share similar characteristics and are stored together.

## Creating Indices

### Basic Index Creation

```json
PUT /my_index
```

### Index with Settings

```json
PUT /products
{
  "settings": {
    "number_of_shards": 3,
    "number_of_replicas": 1,
    "refresh_interval": "30s"
  }
}
```

### Index with Mappings

```json
PUT /users
{
  "mappings": {
    "properties": {
      "username": {
        "type": "keyword"
      },
      "email": {
        "type": "keyword",
        "index": true
      },
      "age": {
        "type": "integer"
      },
      "bio": {
        "type": "text",
        "analyzer": "standard"
      },
      "created_at": {
        "type": "date",
        "format": "yyyy-MM-dd HH:mm:ss||yyyy-MM-dd||epoch_millis"
      }
    }
  }
}
```

## Index Settings

### Static Settings (Set at Creation)

```json
PUT /logs
{
  "settings": {
    "number_of_shards": 5,
    "codec": "best_compression",
    "routing_partition_size": 3
  }
}
```

### Dynamic Settings (Can be Updated)

```json
PUT /logs/_settings
{
  "index": {
    "number_of_replicas": 2,
    "refresh_interval": "5s",
    "max_result_window": 50000,
    "blocks": {
      "read_only": false,
      "write": false
    }
  }
}
```

## Mappings

### Dynamic Mapping

Elasticsearch automatically detects and adds new fields:

```json
PUT /dynamic_index/_doc/1
{
  "title": "Auto-detected as text",
  "count": 42,              // Auto-detected as long
  "price": 19.99,          // Auto-detected as float
  "is_active": true,       // Auto-detected as boolean
  "created": "2024-01-15"  // Auto-detected as date
}
```

### Explicit Mapping

Define field types explicitly:

```json
PUT /products/_mapping
{
  "properties": {
    "name": {
      "type": "text",
      "fields": {
        "keyword": {
          "type": "keyword",
          "ignore_above": 256
        }
      }
    },
    "description": {
      "type": "text",
      "analyzer": "english"
    },
    "price": {
      "type": "scaled_float",
      "scaling_factor": 100
    },
    "tags": {
      "type": "keyword"
    },
    "location": {
      "type": "geo_point"
    }
  }
}
```

### Nested Objects

```json
PUT /orders
{
  "mappings": {
    "properties": {
      "order_id": {
        "type": "keyword"
      },
      "items": {
        "type": "nested",
        "properties": {
          "product_id": {
            "type": "keyword"
          },
          "quantity": {
            "type": "integer"
          },
          "price": {
            "type": "float"
          }
        }
      }
    }
  }
}
```

## Index Templates

### Create Index Template

```json
PUT /_index_template/logs_template
{
  "index_patterns": ["logs-*"],
  "priority": 1,
  "template": {
    "settings": {
      "number_of_shards": 1,
      "number_of_replicas": 0,
      "refresh_interval": "5s"
    },
    "mappings": {
      "properties": {
        "@timestamp": {
          "type": "date"
        },
        "level": {
          "type": "keyword"
        },
        "message": {
          "type": "text"
        }
      }
    }
  }
}
```

### Component Templates

```json
// Create reusable component
PUT /_component_template/common_settings
{
  "template": {
    "settings": {
      "number_of_replicas": 1
    }
  }
}

// Use in index template
PUT /_index_template/app_logs
{
  "index_patterns": ["app-logs-*"],
  "composed_of": ["common_settings"],
  "template": {
    "mappings": {
      "properties": {
        "app_name": {
          "type": "keyword"
        }
      }
    }
  }
}
```

## Index Aliases

### Create Alias

```json
POST /_aliases
{
  "actions": [
    {
      "add": {
        "index": "products_v1",
        "alias": "products"
      }
    }
  ]
}
```

### Filtered Alias

```json
POST /_aliases
{
  "actions": [
    {
      "add": {
        "index": "orders",
        "alias": "recent_orders",
        "filter": {
          "range": {
            "created_at": {
              "gte": "now-7d"
            }
          }
        }
      }
    }
  ]
}
```

### Alias for Zero-Downtime Reindexing

```json
// Step 1: Create new index
PUT /products_v2
{
  "mappings": { /* new mapping */ }
}

// Step 2: Reindex data
POST /_reindex
{
  "source": {
    "index": "products_v1"
  },
  "dest": {
    "index": "products_v2"
  }
}

// Step 3: Switch alias atomically
POST /_aliases
{
  "actions": [
    { "remove": { "index": "products_v1", "alias": "products" } },
    { "add": { "index": "products_v2", "alias": "products" } }
  ]
}
```

## Index Lifecycle Management (ILM)

### Create ILM Policy

```json
PUT /_ilm/policy/logs_policy
{
  "policy": {
    "phases": {
      "hot": {
        "actions": {
          "rollover": {
            "max_age": "7d",
            "max_size": "50gb"
          }
        }
      },
      "warm": {
        "min_age": "7d",
        "actions": {
          "shrink": {
            "number_of_shards": 1
          },
          "forcemerge": {
            "max_num_segments": 1
          }
        }
      },
      "delete": {
        "min_age": "30d",
        "actions": {
          "delete": {}
        }
      }
    }
  }
}
```

### Apply ILM Policy

```json
PUT /logs-000001
{
  "settings": {
    "index.lifecycle.name": "logs_policy",
    "index.lifecycle.rollover_alias": "logs"
  }
}
```

## Index Operations

### Get Index Information

```json
// Get settings
GET /products/_settings

// Get mappings
GET /products/_mapping

// Get both
GET /products

// Get stats
GET /products/_stats
```

### Close and Open Index

```json
// Close index (makes it read-only)
POST /products/_close

// Open index
POST /products/_open
```

### Clone Index

```json
// Put source index in read-only mode
PUT /products/_settings
{
  "settings": {
    "index.blocks.write": true
  }
}

// Clone the index
POST /products/_clone/products_copy
{
  "settings": {
    "index.number_of_replicas": 0
  }
}
```

### Shrink Index

```json
// Prepare for shrink
PUT /logs/_settings
{
  "settings": {
    "index.blocks.write": true
  }
}

// Shrink index
POST /logs/_shrink/logs_shrinked
{
  "settings": {
    "index.number_of_replicas": 1,
    "index.number_of_shards": 1
  }
}
```

## Best Practices

### 1. Naming Conventions

```json
// Use lowercase and descriptive names
PUT /user_profiles     // Good
PUT /UserProfiles     // Bad

// Use versioning for schema changes
PUT /products_v1
PUT /products_v2

// Use date patterns for time-series
PUT /logs-2024.01.15
```

### 2. Shard Sizing

```json
// Calculate shards based on data volume
// Target: 20-40GB per shard

PUT /large_dataset
{
  "settings": {
    "number_of_shards": 10,  // For ~300GB of data
    "number_of_replicas": 1
  }
}
```

### 3. Mapping Optimization

```json
PUT /optimized_index
{
  "mappings": {
    "properties": {
      "exact_match_field": {
        "type": "keyword"      // Don't analyze
      },
      "full_text_field": {
        "type": "text",
        "fields": {
          "keyword": {         // Multi-field for aggregations
            "type": "keyword"
          }
        }
      },
      "numeric_field": {
        "type": "integer",
        "index": false        // Don't index if not searched
      }
    }
  }
}
```

### 4. Index Settings for Performance

```json
PUT /performance_optimized
{
  "settings": {
    "refresh_interval": "30s",           // Reduce refresh frequency
    "number_of_shards": 3,
    "number_of_replicas": 0,             // Add replicas after bulk load
    "index.translog.durability": "async",
    "index.translog.sync_interval": "5s"
  }
}
```

## Monitoring Indices

### Check Index Health

```bash
GET /_cat/indices?v&health=yellow&s=index

GET /_cat/shards/products?v
```

### Index Statistics

```json
GET /products/_stats/store,docs,indexing
```

### Segment Information

```json
GET /products/_segments
```

## Common Issues and Solutions

### 1. Too Many Shards

```json
// Check shard count
GET /_cat/shards?v

// Solution: Use ILM with shrink action
```

### 2. Mapping Explosion

```json
// Limit dynamic fields
PUT /controlled_index
{
  "mappings": {
    "dynamic": "strict",  // Reject unknown fields
    "properties": {
      "known_field": {
        "type": "text"
      }
    }
  }
}
```

### 3. Large Documents

```json
// Set limits
PUT /limited_index
{
  "settings": {
    "index.max_result_window": 10000,
    "index.max_inner_result_window": 100,
    "index.max_terms_count": 65536
  }
}
```

## Next Steps

- Learn about replication strategies
- Understand sharding in detail
- Explore advanced mapping techniques
- Study index performance optimization