CouchDB curl utility provides DELETE method to delete data/document in couchdb database.

Syntax : 
```sh
curl -H 'Content-Type: application/json' \  
-X DELETE http://127.0.0.1:5984/<document>/<doc_id>?rev=2-5fef7ea4661b53c017e167809e4f2beb  
```
First you need to retrieve the document using document_id to get _rev of the document.
```sh
curl -X GET http://127.0.0.1:5984/employee/1

{
    "_id": "1",
    "_rev": "2-fef96a337fdc619lsdfk12owl64a4d2ea",
    "name": "John",
    "city": "California",
    "mobile": 9921829381
}
```
Use the _rev information from above response to delete document in the database

Example :
```sh
curl -X DELETE \
  'http://127.0.0.1:5984/employee/1?rev=2-fef96a337fdc619lsdfk12owl64a4d2ea' \

{
    "ok": true,
    "id": "1",
    "rev": "3-fef96a337f2dab20d11b572ow4a4d2ea"
}

curl -X GET http://127.0.0.1:5984/employee/1

{
    "error": "not_found",
    "reason": "deleted"
}
```
* _rev is required as part of request.
