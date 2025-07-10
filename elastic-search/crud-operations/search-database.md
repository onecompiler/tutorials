# Searching in Elasticsearch

Elasticsearch provides powerful search capabilities through its Search API. This tutorial covers different ways to search data, from simple queries to complex search operations.

## Basic Search

### Search All Documents

```json
GET /products/_search
{
  "query": {
    "match_all": {}
  }
}
```

### URI Search (Query String)

Simple searches using URL parameters:

```bash
# Search all fields
GET /products/_search?q=laptop

# Search specific field
GET /products/_search?q=name:laptop

# Multiple terms
GET /products/_search?q=name:laptop AND category:electronics

# With size limit
GET /products/_search?q=laptop&size=5
```

## Request Body Search

More powerful and flexible than URI search:

### Basic Match Query

```json
GET /products/_search
{
  "query": {
    "match": {
      "description": "wireless keyboard"
    }
  }
}
```

### Exact Term Match

```json
GET /products/_search
{
  "query": {
    "term": {
      "category.keyword": "Electronics"
    }
  }
}
```

## Pagination

### From/Size Pagination

```json
GET /products/_search
{
  "from": 20,
  "size": 10,
  "query": {
    "match_all": {}
  }
}
```

### Search After (for deep pagination)

```json
// First request
GET /products/_search
{
  "size": 10,
  "query": { "match_all": {} },
  "sort": [
    { "created_at": "desc" },
    { "_id": "asc" }
  ]
}

// Subsequent requests
GET /products/_search
{
  "size": 10,
  "query": { "match_all": {} },
  "search_after": [1609459200000, "product_123"],
  "sort": [
    { "created_at": "desc" },
    { "_id": "asc" }
  ]
}
```

## Sorting Results

### Single Field Sort

```json
GET /products/_search
{
  "query": { "match_all": {} },
  "sort": [
    { "price": "asc" }
  ]
}
```

### Multi-Field Sort

```json
GET /products/_search
{
  "query": { "match_all": {} },
  "sort": [
    { "in_stock": "desc" },
    { "price": "asc" },
    { "_score": "desc" }
  ]
}
```

### Sort with Missing Values

```json
GET /products/_search
{
  "sort": [
    {
      "discount": {
        "order": "desc",
        "missing": "_last"
      }
    }
  ]
}
```

## Source Filtering

### Include/Exclude Fields

```json
GET /products/_search
{
  "query": { "match_all": {} },
  "_source": {
    "includes": ["name", "price", "category"],
    "excludes": ["description", "reviews"]
  }
}
```

### Disable Source

```json
GET /products/_search
{
  "query": { "match_all": {} },
  "_source": false
}
```

## Full-Text Search

### Match Query with Options

```json
GET /articles/_search
{
  "query": {
    "match": {
      "content": {
        "query": "elasticsearch tutorial",
        "operator": "and",
        "fuzziness": "AUTO",
        "zero_terms_query": "all"
      }
    }
  }
}
```

### Match Phrase

```json
GET /articles/_search
{
  "query": {
    "match_phrase": {
      "title": "getting started with elasticsearch"
    }
  }
}
```

### Multi-Match Query

```json
GET /products/_search
{
  "query": {
    "multi_match": {
      "query": "laptop computer",
      "fields": ["name^3", "description", "category"],
      "type": "best_fields"
    }
  }
}
```

## Combined Queries

### Bool Query

```json
GET /products/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "name": "laptop"
          }
        }
      ],
      "filter": [
        {
          "range": {
            "price": {
              "gte": 500,
              "lte": 2000
            }
          }
        },
        {
          "term": {
            "in_stock": true
          }
        }
      ],
      "should": [
        {
          "match": {
            "brand": "Apple"
          }
        }
      ],
      "must_not": [
        {
          "term": {
            "category.keyword": "Refurbished"
          }
        }
      ]
    }
  }
}
```

## Highlighting

Highlight matching terms in results:

```json
GET /articles/_search
{
  "query": {
    "match": {
      "content": "elasticsearch"
    }
  },
  "highlight": {
    "fields": {
      "content": {
        "pre_tags": ["<mark>"],
        "post_tags": ["</mark>"],
        "fragment_size": 150,
        "number_of_fragments": 3
      }
    }
  }
}
```

## Search with Aggregations

Combine search with analytics:

```json
GET /products/_search
{
  "query": {
    "match": {
      "category": "laptop"
    }
  },
  "aggs": {
    "price_ranges": {
      "range": {
        "field": "price",
        "ranges": [
          { "to": 1000 },
          { "from": 1000, "to": 2000 },
          { "from": 2000 }
        ]
      }
    },
    "popular_brands": {
      "terms": {
        "field": "brand.keyword",
        "size": 10
      }
    }
  }
}
```

## Fuzzy Search

Handle typos and misspellings:

```json
GET /products/_search
{
  "query": {
    "fuzzy": {
      "name": {
        "value": "lpatop",
        "fuzziness": "AUTO",
        "max_expansions": 50,
        "prefix_length": 2
      }
    }
  }
}
```

## Prefix Search

For autocomplete functionality:

```json
GET /products/_search
{
  "query": {
    "prefix": {
      "name.keyword": {
        "value": "lapt"
      }
    }
  }
}
```

## Wildcard Search

```json
GET /files/_search
{
  "query": {
    "wildcard": {
      "filename.keyword": {
        "value": "report_*.pdf"
      }
    }
  }
}
```

## Phrase Suggester

Get search suggestions:

```json
GET /products/_search
{
  "suggest": {
    "text": "wireles keybord",
    "product_suggestion": {
      "phrase": {
        "field": "name",
        "size": 3,
        "gram_size": 3,
        "confidence": 1.0,
        "max_errors": 2
      }
    }
  }
}
```

## Scroll API (Deprecated, use search_after)

For retrieving large result sets:

```json
// Initial request
POST /products/_search?scroll=1m
{
  "size": 100,
  "query": {
    "match_all": {}
  }
}

// Subsequent requests
POST /_search/scroll
{
  "scroll": "1m",
  "scroll_id": "DXF1ZXJ5QW5kRmV0Y2gBAAAAAAAAAD4WYm9laVYtZndUQlNsdDcwakFMNjU1QQ=="
}
```

## Count API

Get document count without results:

```json
GET /products/_count
{
  "query": {
    "range": {
      "price": {
        "gte": 100
      }
    }
  }
}
```

## Explain API

Understand why a document matched:

```json
GET /products/_explain/1
{
  "query": {
    "match": {
      "name": "laptop"
    }
  }
}
```

## Search Templates

Reusable search queries:

```json
// Create template
PUT _scripts/product_search
{
  "script": {
    "lang": "mustache",
    "source": {
      "query": {
        "match": {
          "name": "{{search_term}}"
        }
      },
      "size": "{{size}}"
    }
  }
}

// Use template
GET /products/_search/template
{
  "id": "product_search",
  "params": {
    "search_term": "laptop",
    "size": 10
  }
}
```

## Best Practices

### 1. Use Filters for Non-Scoring Queries

```json
{
  "query": {
    "bool": {
      "must": { "match": { "title": "elasticsearch" } },
      "filter": { "term": { "status": "published" } }
    }
  }
}
```

### 2. Limit Response Size

```json
{
  "query": { "match_all": {} },
  "size": 10,
  "_source": ["name", "price"]
}
```

### 3. Use Appropriate Query Types

- `term`: Exact matching
- `match`: Full-text search
- `range`: Numeric/date ranges
- `bool`: Complex combinations

### 4. Optimize for Performance

```json
{
  "query": {
    "bool": {
      "filter": [  // Use filter context when possible
        { "term": { "category": "electronics" } },
        { "range": { "price": { "lte": 1000 } } }
      ]
    }
  },
  "track_total_hits": false  // Disable if exact count not needed
}
```

## Common Search Patterns

### E-commerce Product Search

```json
GET /products/_search
{
  "query": {
    "bool": {
      "must": {
        "multi_match": {
          "query": "gaming laptop",
          "fields": ["name^3", "description", "tags"]
        }
      },
      "filter": [
        { "range": { "price": { "gte": 500, "lte": 2000 } } },
        { "term": { "in_stock": true } }
      ]
    }
  },
  "sort": [
    { "_score": "desc" },
    { "popularity": "desc" }
  ],
  "aggs": {
    "brands": {
      "terms": { "field": "brand.keyword" }
    }
  }
}
```

### Log Search

```json
GET /logs/_search
{
  "query": {
    "bool": {
      "must": [
        { "match": { "message": "error" } }
      ],
      "filter": [
        { "term": { "level": "ERROR" } },
        { "range": { "@timestamp": { "gte": "now-1h" } } }
      ]
    }
  },
  "sort": [
    { "@timestamp": "desc" }
  ]
}
```

## Next Steps

- Learn about Query DSL in detail
- Explore aggregations for analytics
- Understand relevance scoring
- Study search performance optimization