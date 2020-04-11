## Update API with script
This update API used to update a document based on a script provided

Below update request adds new field to document: 
```json
POST databases/_update/1
{
    "script" : "ctx._source.rank = 4"
}
```
Below update request will increment the value of rank field with the value mentioned in `params` :
```json
POST databases/_update/1
{
    "script" : {
        "source": "ctx._source.rank += params.count",
        "lang": "painless",
        "params" : {
            "count" : 2
        }
    }
}
```

## Update API with doc
This update API is used to pass partial document and merge it into existing document

Below update request adds a new field to existing document:
```json
POST databases/_update/1
{
    "doc" : {
        "written_in" : "java"
    }
}
```
