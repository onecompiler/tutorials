CouchDB curl utility provides PUT method to insert data/document into couchdb database.

Syntax : 
```sh
curl -X PUT http://127.0.0.1:5984/<database>/"id" -d ' { document } '   
```
Example :
```sh
curl -X PUT \
  http://127.0.0.1:5984/employee/1 \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "John",
    "city": "New york",
    "mobile": 9921829381
}'

{
    "ok": true,
    "id": "1",
    "rev": "1-fef96a337fdc6197293y41fs64a4d2ea"
}
```
Document id, datbase name needs to be specified in curl request.

Document can be displayed in the following manner.

```sh
curl -X GET http://127.0.0.1:5984/employee/1

{
    "_id": "1",
    "rev": "1-fef96a337fdc6197293y41fs64a4d2ea",
    "name": "John",
    "city": "New york",
    "mobile": 9921829381
}
```

