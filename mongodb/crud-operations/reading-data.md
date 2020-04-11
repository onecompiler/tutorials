## Reading all documents from a collection
`find` is used to read documents from a MongoDB collection. This takes a query as input. If an empty query provided then it returns all the documents from a collection. 

```javascript
db.cars.find({});
```

## Reading documents with matching condition
If a query passed to find method, MongoDB returns documents matching that filter condition

```javascript
db.cars.find({ type: "SUV" });
```

## Reading a single document using `findOne`
`findOne` is used to return a single document matching the filter condition

```javascript
db.cars.find({ type: "SUV" });
```

## Matching multiple values
`in` operater is used to match multiple values

```javascript
db.cars.find({ type: {$in : ["SUV", "Sedan"]} });
```

