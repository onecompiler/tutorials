Aggregations in MongoDB are used to process records and return computed results. There are three ways of performing aggregation operations in MongoDB

1. Aggregation Pipeline
2. Map-Reduce
3. Single Purpose Aggregation Operations


## Aggregation Pipeline
In Aggregation Pipelines data enters through multi-stage pipeline and the data gets transfomed into an aggregated result. 

## Map-Reduce
Map-Reduce operations have two phases first is a mapping stage which takes a document and transform it to another with the given inputs, then the reduce stage that combines the output of Map stage

## Single Purpose Aggregation Operations
MongoDB provides three simple to use Single Purpose Aggregation Operations

1. estimatedDocumentCount
2. count
3. distinct

Examples: 
```javascript
db.cars.count(); // returns the total count of documents from cars collection
db.cars.distinct('type'); // returns all distinct values if type Ex. SUV, Sedan, Hatchback
```