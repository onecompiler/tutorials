CouchDB curl utility provides PUT method to update data/document in couchdb database.

Syntax : 
```sh
curl -X PUT http://127.0.0.1:5984/<database>/<document_id>/ -d '{ "field" : "value", "_rev" : "revision id" }'  
```
First you need to retrieve the document using document_id to get _rev of the document.
```sh
curl -X GET http://127.0.0.1:5984/employee/1

{
    "_id": "1",
    "_rev": "1-fef96a337k65197245f984558a4d2ea",
    "name": "John",
    "city": "New york",
    "mobile": 9921829381
}
```
Use the _rev information from above response to update data in the document

Example :
```sh
curl -X PUT \
  http://127.0.0.1:5984/employee/1 \
  -H 'Content-Type: application/json' \
  -d '{
    "_rev": "1-fef96a337k65197245f984558a4d2ea",
    "name": "John",
    "city": "California",
    "mobile": 9921829381
}'

{
    "ok": true,
    "id": "1",
    "rev": "1-fef96a337fdc619lsdfk12owl64a4d2ea"
}
```
* _rev is required as part of request.
* You can only write entirely new version of document i.e you need to specify all the fields even though you want to add single field.