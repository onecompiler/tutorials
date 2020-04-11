## Delete API
This API will delete the document from the `databases` index with ID 1:
```json
DELETE /databases/_doc/1
```
The result of above request:
```json
{
    "_shards" : {
        "total" : 2,
        "failed" : 0,
        "successful" : 2
    },
    "_index" : "databases",
    "_type" : "_doc",
    "_id" : "1",
    "_version" : 2,
    "_primary_term": 1,
    "_seq_no": 5,
    "result": "deleted"
}
```

## Delete by Query API
This `delete_by_query` API deletes every document that matches with given query 
```json
POST databases/_delete_by_query
{
  "query": { 
    "match": {
      "name": "Elastic Search"
    }
  }
}
```
The result of above request:
```json
{
  "took" : 14,
  "timed_out": false,
  "deleted": 2,
  "batches": 1,
  "version_conflicts": 0,
  "noops": 0,
  "retries": {
    "bulk": 0,
    "search": 0
  },
  "throttled_millis": 0,
  "requests_per_second": -1.0,
  "throttled_until_millis": 0,
  "total": 2,
  "failures" : [ ]
}
```