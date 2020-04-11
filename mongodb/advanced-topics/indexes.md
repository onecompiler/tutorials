Indexes are used to run queries more efficiently in MongoDB. 

## Creating Indexes
For example you are going to make queries based on `companyName` in your queries then you can create an index on `companyName` field. 

```javascript
db.collection.createIndex( { companyName: 1 } )
```

## Index Types
MongoDB provides different types of indexes to support different usecases. Following are the different types of indexes MongoDB provides

1. Single Field - ascending/descending indexes on a single field
2. Compound Index - to create indexes on multiple fields
3. Multikey Index - to create indexes on array type fields 
4. Geospatial Index - to perform queries on geospatial data
5. Text Indexes - For text search
6. Hashed Indexes - For hash based sharding
