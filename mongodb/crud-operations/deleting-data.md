`deleteOne` and `deleteMany` are used to delete existing documents in MongoDB.

## Deleting a single document
`deleteOne` is used to delete a single document in a collection

#### Syntax

```javascript
db.cars.deleteOne({
    <query>
});
```

#### Example

```javascript
db.cars.deleteOne({
    {
        companyName: "Volvo",
        model: "XC-90"
    }
});
```


## Deleting multiple documents

`deleteMany` is used to delete multiple documents in a collection

#### Syntax

```javascript
db.cars.deleteMany({
    <query>
});
```

#### Example

```javascript
db.cars.deleteMany({
    {
        companyName: "Volvo"
    }
});
```