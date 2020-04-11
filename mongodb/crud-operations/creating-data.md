### Inserting a single Document

We can use `insertOne()` to insert a single Document.

```javascript
db.cars.insertOne({
    companyName: "Volvo",
    model: "XC-90",
    type: "SUV",
    year: "2019",
    colors: ["black", "white", "silver"],
    specifications: {
        length: 4950,
        width: 2140,
        height: 1776
    }
})
```

### Inserting Multiple Documents

`insertMany()` is used to insert multiple documents 

```javascript
db.variants.insertMany([
   {companyName: "Volvo", model: "XC-90", variants: "T8 Inscription"},
   {companyName: "Volvo", model: "XC-90", variants: "T8 Excellence"}
])
```

### The `_id` field

MongoDB needs a unique `_id` field for each and every document we save to a collection. If user does not provide a `_id` field then MongoDB generates an unique ObjectId for the `_id` field.


### Deprecated `save` Method
MongoDB also have save method to insert/ update documents. This need to avoided as its deprecated
