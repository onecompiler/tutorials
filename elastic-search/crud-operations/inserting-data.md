## Create Document with ID
This index API adds or updates the JSON document into the `databases` index with an id of 1 :
```json
PUT databases/_doc/1
{
    "name" : "Elastic Search",
    "type" : "Document-Oriented Database"
}
```
The result of above request :
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
    "_version" : 1,
    "_seq_no" : 0,
    "_primary_term" : 1,
    "result" : "created"
}
```
## Create Document with Operation Type
This index API used to force `create` operation on document,which will fail if a document by that id already exists in the index

>Using `op_type` parameter
```json
PUT databases/_doc/1?op_type=create
{
    "name" : "Elastic Search",
    "type" : "Document-Oriented Database"
}
```
>Without `op_type` parameter
```json
PUT databases/_create/1
{
    "name" : "Elastic Search",
    "type" : "Document-Oriented Database"
}
```
## Create Document with Automatic ID Generation
This index API adds the JSON document into the `databases` index with an autmatically generated id and also follow `create` operation as default
```json
POST databases/_doc/
{
    "name" : "Elastic Search",
    "type" : "Document-Oriented Database"
}
```
The result of above request :
```json
{
    "_shards" : {
        "total" : 2,
        "failed" : 0,
        "successful" : 2
    },
    "_index" : "databases",
    "_type" : "_doc",
    "_id" : "1WEKA2SMDA0912",
    "_version" : 1,
    "_seq_no" : 0,
    "_primary_term" : 1,
    "result" : "created"
}
```



