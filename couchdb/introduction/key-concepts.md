## Document Oriented
Document oriented databases are different from traditional RDBMS. 
Document oriented databases contain documents these are like rows in RDBMS which stores the data. These documents can be complex and have multilevel data. For example, a user document can have a list of roles which is an array of Strings and also can contain address which is another object with its own fields like the dollowing example 

```json
{
    "id": 1,
    "name": "Tom",
    "roles": ["admin", "manager"],
    "address" : {
        "street": "xxxx",
        "zipcode": 12345,
        "country": "US"
    }
}
```
## Replication
CouchDB provides the simplest form of replication.

## Authentication
CouchDB can make authentication open through a session cookie like web application.

## CouchDB Fauxton
CouchDB Fauxton is built-in web based administrative interface to access database. CRUD operations can be easily performed through this system.

## CouchDB Curl
CouchDB Curl is another utility to interact with CouchDB database. This tool is used to transfer data from or to server by using one of supported protocols(HTTP, HTTPS, FTP). In this tutorial we will be using CouchDB curl using HTTP protocol to explain different operations/commands in CouchDB.

