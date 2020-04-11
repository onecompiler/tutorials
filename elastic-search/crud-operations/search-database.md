## Search API with query parameter
This search API is used to execute a search query on the index given and get back results that match the query

Below request will search for Elastic search name in `databases` index:
```json
GET /databases/_search?q=name:Elastic search
```

## Search API with request body
This search API is used to execute a Query DSL on the index given and get back results that match the query

Below request will search for Elastic search name in `databases` index:
```json
GET /databases/_search
{
    "query" : {
        "term" : { "name" : "Elastic search" }
    }
}
```