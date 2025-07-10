# Query DSL Basics

Elasticsearch provides a full Query DSL (Domain Specific Language) based on JSON to define queries. This tutorial covers the fundamental query types and how to use them effectively.

## Query Context vs Filter Context

### Query Context
Answers "How well does this document match this query?"
- Calculates relevance score
- Results are ranked by score
- Not cached

### Filter Context
Answers "Does this document match this query?"
- Yes/no answer
- No scoring
- Cached for performance

Example showing both contexts:
```json
GET /products/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "description": "laptop"  // Query context - scored
          }
        }
      ],
      "filter": [
        {
          "range": {
            "price": {
              "gte": 100,
              "lte": 1000     // Filter context - not scored
            }
          }
        }
      ]
    }
  }
}
```

## Match Queries

### Match Query

The standard query for full-text search:

```json
GET /products/_search
{
  "query": {
    "match": {
      "description": "wireless mouse"
    }
  }
}
```

### Match with Options

```json
GET /products/_search
{
  "query": {
    "match": {
      "description": {
        "query": "wireless mouse",
        "operator": "and",           // All terms must match
        "fuzziness": "AUTO",        // Allow typos
        "zero_terms_query": "all"   // Behavior when all terms removed by analyzer
      }
    }
  }
}
```

### Match Phrase

Matches exact phrases:

```json
GET /articles/_search
{
  "query": {
    "match_phrase": {
      "content": "machine learning"
    }
  }
}
```

### Match Phrase with Slop

Allows words to be further apart:

```json
GET /articles/_search
{
  "query": {
    "match_phrase": {
      "content": {
        "query": "machine learning",
        "slop": 2  // Allows up to 2 words between
      }
    }
  }
}
```

### Multi Match

Search multiple fields:

```json
GET /products/_search
{
  "query": {
    "multi_match": {
      "query": "laptop bag",
      "fields": ["name^3", "description", "tags"],  // ^3 boosts name field
      "type": "best_fields"  // Options: best_fields, most_fields, cross_fields, phrase
    }
  }
}
```

## Term-Level Queries

### Term Query

Exact term matching (not analyzed):

```json
GET /users/_search
{
  "query": {
    "term": {
      "status.keyword": "active"
    }
  }
}
```

### Terms Query

Match any of the exact values:

```json
GET /products/_search
{
  "query": {
    "terms": {
      "category.keyword": ["Electronics", "Computers", "Tablets"]
    }
  }
}
```

### Range Query

```json
GET /products/_search
{
  "query": {
    "range": {
      "price": {
        "gte": 10,
        "lt": 100,
        "boost": 2.0
      }
    }
  }
}
```

Date ranges:
```json
GET /logs/_search
{
  "query": {
    "range": {
      "timestamp": {
        "gte": "2024-01-01",
        "lte": "now",
        "time_zone": "+01:00"
      }
    }
  }
}
```

### Exists Query

Find documents with a field:

```json
GET /products/_search
{
  "query": {
    "exists": {
      "field": "discount"
    }
  }
}
```

### Prefix Query

```json
GET /users/_search
{
  "query": {
    "prefix": {
      "username.keyword": "john"
    }
  }
}
```

### Wildcard Query

```json
GET /files/_search
{
  "query": {
    "wildcard": {
      "filename.keyword": "*.pdf"
    }
  }
}
```

### Fuzzy Query

Handles typos:

```json
GET /products/_search
{
  "query": {
    "fuzzy": {
      "name": {
        "value": "iphon",
        "fuzziness": "AUTO",
        "max_expansions": 50
      }
    }
  }
}
```

## Boolean Queries

Combine multiple queries:

### Must, Should, Must Not, Filter

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
      "should": [
        {
          "match": {
            "brand": "Apple"
          }
        },
        {
          "match": {
            "brand": "Dell"
          }
        }
      ],
      "must_not": [
        {
          "range": {
            "price": {
              "gt": 2000
            }
          }
        }
      ],
      "filter": [
        {
          "term": {
            "in_stock": true
          }
        }
      ],
      "minimum_should_match": 1
    }
  }
}
```

## Nested Queries

For nested objects:

```json
GET /orders/_search
{
  "query": {
    "nested": {
      "path": "items",
      "query": {
        "bool": {
          "must": [
            {
              "match": {
                "items.product_name": "iPhone"
              }
            },
            {
              "range": {
                "items.quantity": {
                  "gte": 2
                }
              }
            }
          ]
        }
      }
    }
  }
}
```

## Geo Queries

### Geo Distance

Find documents within a distance:

```json
GET /stores/_search
{
  "query": {
    "geo_distance": {
      "distance": "10km",
      "location": {
        "lat": 40.7128,
        "lon": -74.0060
      }
    }
  }
}
```

### Geo Bounding Box

```json
GET /stores/_search
{
  "query": {
    "geo_bounding_box": {
      "location": {
        "top_left": {
          "lat": 40.73,
          "lon": -74.1
        },
        "bottom_right": {
          "lat": 40.01,
          "lon": -71.12
        }
      }
    }
  }
}
```

## Query String

Lucene query syntax:

```json
GET /articles/_search
{
  "query": {
    "query_string": {
      "query": "(title:elasticsearch OR title:lucene) AND status:published",
      "default_field": "content"
    }
  }
}
```

Simpler version:
```json
GET /articles/_search
{
  "query": {
    "simple_query_string": {
      "query": "elasticsearch + \"search engine\" -solr",
      "fields": ["title", "content"]
    }
  }
}
```

## Boosting Queries

### Boosting Query

Demote certain results:

```json
GET /articles/_search
{
  "query": {
    "boosting": {
      "positive": {
        "match": {
          "content": "elasticsearch"
        }
      },
      "negative": {
        "term": {
          "status": "archived"
        }
      },
      "negative_boost": 0.5
    }
  }
}
```

### Function Score

Custom scoring:

```json
GET /products/_search
{
  "query": {
    "function_score": {
      "query": {
        "match": {
          "name": "laptop"
        }
      },
      "functions": [
        {
          "filter": {
            "term": {
              "brand": "Apple"
            }
          },
          "weight": 2
        },
        {
          "gauss": {
            "price": {
              "origin": "500",
              "scale": "100"
            }
          }
        }
      ],
      "boost_mode": "multiply"
    }
  }
}
```

## Highlighting

Highlight matching terms:

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
        "pre_tags": ["<strong>"],
        "post_tags": ["</strong>"],
        "fragment_size": 150,
        "number_of_fragments": 3
      }
    }
  }
}
```

## Query Validation

Validate queries before execution:

```json
GET /products/_validate/query?explain=true
{
  "query": {
    "match": {
      "description": "laptop computer"
    }
  }
}
```

## Performance Tips

### 1. Use Filters When Possible

```json
// Good - uses filter for non-scoring criteria
{
  "bool": {
    "must": { "match": { "title": "elasticsearch" } },
    "filter": { "term": { "status": "published" } }
  }
}
```

### 2. Limit Fields

```json
GET /products/_search
{
  "_source": ["name", "price"],  // Only return needed fields
  "query": { "match_all": {} }
}
```

### 3. Use Query-Time Boosting Carefully

```json
// Be careful with field boosting
{
  "multi_match": {
    "query": "search term",
    "fields": ["title^10", "content"]  // Very high boost
  }
}
```

### 4. Profile Queries

```json
GET /products/_search
{
  "profile": true,
  "query": {
    "match": {
      "description": "laptop"
    }
  }
}
```

## Common Patterns

### Search with Filters

```json
GET /products/_search
{
  "query": {
    "bool": {
      "must": {
        "multi_match": {
          "query": "laptop bag",
          "fields": ["name", "description"]
        }
      },
      "filter": [
        { "term": { "in_stock": true } },
        { "range": { "price": { "lte": 100 } } },
        { "terms": { "brand.keyword": ["Samsonite", "Targus"] } }
      ]
    }
  }
}
```

### Autocomplete Pattern

```json
GET /products/_search
{
  "query": {
    "match_phrase_prefix": {
      "name": {
        "query": "lapt",
        "max_expansions": 10
      }
    }
  }
}
```

### Faceted Search

```json
GET /products/_search
{
  "query": {
    "match": { "description": "laptop" }
  },
  "aggs": {
    "brands": {
      "terms": { "field": "brand.keyword" }
    },
    "price_ranges": {
      "range": {
        "field": "price",
        "ranges": [
          { "to": 500 },
          { "from": 500, "to": 1000 },
          { "from": 1000 }
        ]
      }
    }
  }
}
```

## Next Steps

- Learn about advanced scoring and relevance
- Explore specialized queries (more_like_this, percolate)
- Understand query optimization techniques
- Study aggregations with queries