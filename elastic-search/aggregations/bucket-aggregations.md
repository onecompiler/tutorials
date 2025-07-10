# Bucket Aggregations in Elasticsearch

Bucket aggregations create buckets of documents where each bucket is associated with a key and a document count. Think of them as GROUP BY operations in SQL, but much more powerful.

## Overview

Bucket aggregations don't calculate metrics over fields like metrics aggregations do. Instead, they create buckets of documents. Each bucket is associated with a criterion (depends on the aggregation type) that determines whether a document falls into it.

## Terms Aggregation

The most common bucket aggregation, groups documents by unique field values.

### Basic Terms Aggregation

```json
GET /products/_search
{
  "size": 0,
  "aggs": {
    "product_categories": {
      "terms": {
        "field": "category.keyword"
      }
    }
  }
}
```

Response:
```json
{
  "aggregations": {
    "product_categories": {
      "buckets": [
        {
          "key": "Electronics",
          "doc_count": 150
        },
        {
          "key": "Clothing",
          "doc_count": 120
        },
        {
          "key": "Books",
          "doc_count": 80
        }
      ]
    }
  }
}
```

### Terms with Size and Order

```json
GET /products/_search
{
  "size": 0,
  "aggs": {
    "top_brands": {
      "terms": {
        "field": "brand.keyword",
        "size": 20,
        "order": { "_count": "desc" }
      }
    }
  }
}
```

### Multi-field Terms

```json
GET /sales/_search
{
  "size": 0,
  "aggs": {
    "sales_by_region": {
      "terms": {
        "field": "region.keyword"
      },
      "aggs": {
        "total_sales": {
          "sum": {
            "field": "amount"
          }
        }
      }
    }
  }
}
```

## Date Histogram Aggregation

Groups documents into time-based buckets.

### Basic Date Histogram

```json
GET /logs/_search
{
  "size": 0,
  "aggs": {
    "logs_over_time": {
      "date_histogram": {
        "field": "timestamp",
        "calendar_interval": "day"
      }
    }
  }
}
```

### Date Histogram with Custom Interval

```json
GET /sales/_search
{
  "size": 0,
  "aggs": {
    "monthly_sales": {
      "date_histogram": {
        "field": "date",
        "calendar_interval": "month",
        "format": "yyyy-MM",
        "min_doc_count": 0,
        "extended_bounds": {
          "min": "2024-01-01",
          "max": "2024-12-31"
        }
      },
      "aggs": {
        "revenue": {
          "sum": {
            "field": "amount"
          }
        }
      }
    }
  }
}
```

### Fixed Interval

```json
GET /metrics/_search
{
  "size": 0,
  "aggs": {
    "metrics_per_hour": {
      "date_histogram": {
        "field": "timestamp",
        "fixed_interval": "30m",
        "time_zone": "America/New_York"
      }
    }
  }
}
```

## Range Aggregation

Creates arbitrary ranges of numeric or date values.

### Numeric Range

```json
GET /products/_search
{
  "size": 0,
  "aggs": {
    "price_ranges": {
      "range": {
        "field": "price",
        "ranges": [
          { "to": 50, "key": "Cheap" },
          { "from": 50, "to": 200, "key": "Moderate" },
          { "from": 200, "key": "Expensive" }
        ]
      }
    }
  }
}
```

### Date Range Aggregation

```json
GET /users/_search
{
  "size": 0,
  "aggs": {
    "user_age_groups": {
      "date_range": {
        "field": "birth_date",
        "format": "yyyy",
        "ranges": [
          { "key": "Gen Z", "from": "2000", "to": "2010" },
          { "key": "Millennials", "from": "1980", "to": "2000" },
          { "key": "Gen X", "from": "1965", "to": "1980" }
        ]
      }
    }
  }
}
```

## Histogram Aggregation

Creates fixed-size buckets over numeric values.

```json
GET /products/_search
{
  "size": 0,
  "aggs": {
    "price_histogram": {
      "histogram": {
        "field": "price",
        "interval": 50,
        "min_doc_count": 0,
        "extended_bounds": {
          "min": 0,
          "max": 500
        }
      }
    }
  }
}
```

## Filter Aggregation

Creates a single bucket matching a query.

### Single Filter

```json
GET /products/_search
{
  "size": 0,
  "aggs": {
    "electronics_only": {
      "filter": {
        "term": { "category": "Electronics" }
      },
      "aggs": {
        "avg_price": {
          "avg": { "field": "price" }
        }
      }
    }
  }
}
```

### Filters Aggregation (Multiple Filters)

```json
GET /logs/_search
{
  "size": 0,
  "aggs": {
    "log_levels": {
      "filters": {
        "filters": {
          "errors": { "term": { "level": "ERROR" } },
          "warnings": { "term": { "level": "WARN" } },
          "info": { "term": { "level": "INFO" } }
        }
      },
      "aggs": {
        "top_messages": {
          "terms": {
            "field": "message.keyword",
            "size": 5
          }
        }
      }
    }
  }
}
```

## Composite Aggregation

Useful for paginating through large sets of buckets.

```json
GET /sales/_search
{
  "size": 0,
  "aggs": {
    "sales_composite": {
      "composite": {
        "size": 10,
        "sources": [
          {
            "product": {
              "terms": {
                "field": "product_id"
              }
            }
          },
          {
            "date": {
              "date_histogram": {
                "field": "timestamp",
                "calendar_interval": "day"
              }
            }
          }
        ]
      },
      "aggs": {
        "total_sales": {
          "sum": {
            "field": "amount"
          }
        }
      }
    }
  }
}
```

### Pagination with After Key

```json
GET /sales/_search
{
  "size": 0,
  "aggs": {
    "sales_composite": {
      "composite": {
        "size": 10,
        "sources": [
          { "product": { "terms": { "field": "product_id" } } }
        ],
        "after": {
          "product": "PROD-123"
        }
      }
    }
  }
}
```

## Nested Aggregation

For aggregating on nested documents.

```json
GET /products/_search
{
  "size": 0,
  "aggs": {
    "reviews": {
      "nested": {
        "path": "reviews"
      },
      "aggs": {
        "avg_rating": {
          "avg": {
            "field": "reviews.rating"
          }
        },
        "rating_histogram": {
          "histogram": {
            "field": "reviews.rating",
            "interval": 1
          }
        }
      }
    }
  }
}
```

## Global Aggregation

Breaks out of the current aggregation context.

```json
GET /sales/_search
{
  "query": {
    "term": { "category": "Electronics" }
  },
  "size": 0,
  "aggs": {
    "electronics_avg": {
      "avg": { "field": "price" }
    },
    "all_products": {
      "global": {},
      "aggs": {
        "all_avg": {
          "avg": { "field": "price" }
        }
      }
    }
  }
}
```

## Significant Terms Aggregation

Finds uncommonly common terms.

```json
GET /articles/_search
{
  "query": {
    "match": { "category": "technology" }
  },
  "size": 0,
  "aggs": {
    "significant_words": {
      "significant_terms": {
        "field": "content.keyword",
        "size": 10
      }
    }
  }
}
```

## Combining Multiple Bucket Aggregations

### Nested Buckets

```json
GET /sales/_search
{
  "size": 0,
  "aggs": {
    "by_category": {
      "terms": {
        "field": "category.keyword"
      },
      "aggs": {
        "by_brand": {
          "terms": {
            "field": "brand.keyword"
          },
          "aggs": {
            "price_stats": {
              "stats": {
                "field": "price"
              }
            }
          }
        }
      }
    }
  }
}
```

### Time-based Analysis

```json
GET /orders/_search
{
  "size": 0,
  "aggs": {
    "sales_per_day": {
      "date_histogram": {
        "field": "order_date",
        "calendar_interval": "day"
      },
      "aggs": {
        "hourly_distribution": {
          "histogram": {
            "field": "hour_of_day",
            "interval": 1,
            "min_doc_count": 0
          }
        },
        "revenue": {
          "sum": {
            "field": "total_amount"
          }
        }
      }
    }
  }
}
```

## Best Practices

### 1. Use keyword Fields for Terms Aggregations

```json
// Good
"terms": { "field": "category.keyword" }

// Bad (will fail on text fields)
"terms": { "field": "category" }
```

### 2. Control Memory Usage

```json
{
  "terms": {
    "field": "user_id",
    "size": 100,  // Limit bucket count
    "shard_size": 200  // Control shard-level precision
  }
}
```

### 3. Handle Missing Values

```json
{
  "terms": {
    "field": "category.keyword",
    "missing": "Unknown"  // Include docs with missing values
  }
}
```

### 4. Optimize Date Histograms

```json
{
  "date_histogram": {
    "field": "timestamp",
    "calendar_interval": "day",
    "min_doc_count": 0,  // Include empty buckets
    "offset": "+6h"      // Adjust bucket boundaries
  }
}
```

## Common Use Cases

### 1. E-commerce Analytics

```json
GET /orders/_search
{
  "size": 0,
  "aggs": {
    "daily_orders": {
      "date_histogram": {
        "field": "order_date",
        "calendar_interval": "day"
      },
      "aggs": {
        "unique_customers": {
          "cardinality": {
            "field": "customer_id"
          }
        },
        "revenue": {
          "sum": {
            "field": "total"
          }
        }
      }
    }
  }
}
```

### 2. Log Analysis

```json
GET /logs/_search
{
  "size": 0,
  "aggs": {
    "error_timeline": {
      "filter": {
        "term": { "level": "ERROR" }
      },
      "aggs": {
        "errors_per_hour": {
          "date_histogram": {
            "field": "@timestamp",
            "fixed_interval": "1h"
          }
        }
      }
    }
  }
}
```

### 3. User Behavior Analysis

```json
GET /events/_search
{
  "size": 0,
  "aggs": {
    "user_sessions": {
      "terms": {
        "field": "session_id.keyword",
        "size": 1000
      },
      "aggs": {
        "session_duration": {
          "max": { "field": "timestamp" }
        },
        "events_count": {
          "value_count": { "field": "event_type" }
        }
      }
    }
  }
}
```

## Performance Tips

1. **Limit bucket count**: Use `size` parameter wisely
2. **Use doc_count ordering**: Default and most efficient
3. **Filter first**: Apply queries to reduce document set
4. **Use composite aggregation**: For large result sets requiring pagination
5. **Cache aggregations**: Frequently used aggregations benefit from caching

## Next Steps

- Combine bucket and metrics aggregations
- Learn about pipeline aggregations
- Explore sub-aggregations patterns
- Study aggregation performance optimization