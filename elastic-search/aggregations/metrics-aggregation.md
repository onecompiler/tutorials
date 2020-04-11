## Avg Aggregation
Avg Aggregation is a single-value metrics aggregation which computes the average of all numeric values of a field from aggregated documents

Below request will compute the average rank of all documents in `databases` index:
```json
POST /databases/_search?size=0
{
    "aggs" : {
        "avg_rank" : { "avg" : { "field" : "rank" } }
    }
}
```

## Max Aggregation
Max Aggregation is a single-value metrics aggregation which returns the maximum of all numeric values of a field from aggregated documents

Below request will return the maximum rank of all documents in `databases` index:
```json
POST /databases/_search?size=0
{
    "aggs" : {
        "max_rank" : { "max" : { "field" : "rank" } }
    }
}
```

## Min Aggregation
Min Aggregation is a single-value metrics aggregation which returns the minimum of all numeric values of a field from aggregated documents

Below request will return the minimum rank of all documents in `databases` index:
```json
POST /databases/_search?size=0
{
    "aggs" : {
        "min_rank" : { "min" : { "field" : "rank" } }
    }
}
```

## Sum Aggregation
Sum Aggregation is a single-value metrics aggregation which returns the sum of all numeric values of a field from aggregated documents

Below request will return the sum of all rank fields in `databases` index:
```json
POST /databases/_search?size=0
{
    "aggs" : {
        "sum_rank" : { "sum" : { "field" : "rank" } }
    }
}
```



