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
## No Schema
Mongo DB is a schema less database, that means user don't have to define a schema before start inserting the data. And also it is flexible i.e we can introduce new fields without defining them anywhere.

## database, collection, fields
Just like any other database users can create multiple databases in MongoDB, Database contains collections, collections contains documents. The document is made up of fields and its data.

## Mongo Shell 
Mongo shell is a utility provided by MongoDB. It is a CLI interface to interact with MongoDB.
