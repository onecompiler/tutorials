`updateOne` and `updateMany` are used to update existing documents in MongoDB.

## Updating a single document
`updateOne` is used to update a single document in a collection

#### Syntax

```javascript
db.cars.updateOne({
    <query>,
    <updated_data>
});
```

#### Example

```javascript
db.cars.updateOne({
    {
        companyName: "Volvo",
        model: "XC-90"
    },
    {
        $set: {type: "SUV"}
    }
});
```

Note: `$set` operator updates the given fields to the new values

## Updating multiple documents

`updateMany` is used to update multiple documents in a collection

#### Syntax

```javascript
db.cars.updateMany({
    <query>,
    <updated_data>
});
```

#### Example

```javascript
db.cars.updateMany({
    {
        companyName: "Volvo"
    },
    {
        $set: { type: "SUV" }
    }
});
```