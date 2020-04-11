## Get API
The Get API is used to get JSON document from `databases` index with id value 1:
```json
GET databases/_doc/1
```
The result of above operation:
```json
{
    "_index" : "databases",
    "_type" : "_doc",
    "_id" : "1",
    "_version" : 1,
    "_seq_no" : 10,
    "_primary_term" : 1,
    "found": true,
    "_source" : {
       "name" : "Elastic Search",
       "type" : "Document-Oriented Database"
    }
}
```

