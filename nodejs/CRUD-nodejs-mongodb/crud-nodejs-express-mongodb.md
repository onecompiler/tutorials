## Pre-requisites
* Familiar with Node js and MongoDB and are installed in your machine. 
* You can use Visual studio code to develop your projects.
* Install Postman to test your code

## Terminology

### Express
Express is a framework which runs on top of Node js for building web applications and it is more preferable as it simplifies server creation process

### MongoDB
MongoDB is a no-sql database which is used widely now a days.

### CRUD
CRUD is a acronym for Create, read, update and delete operations

## How to start

1. create a folder in your repository 
2. Open terminal and navigate to the folder you created.
3. execute `npm init`, which creates package.json file.
4. execute `code package.json`, to open the package.json in your visual studio code.

## Dependencies

* Install below dependencies

```javascript
npm i express mongodb body-parser --save
```

## Code

Consider pets is a collection which has id, name and age information of pets in your local machine. You can perform CRUD operations on pets as below

``` javascript
const express = require('express');
const server = express();

var bodyParser = require('body-parser');

server.use(bodyParser.urlencoded({
    limit: "4mb",
    extended: false
}));
server.use(bodyParser.json({ limit: "4mb" }));

const MongoClient = require('mongodb').MongoClient;
const url = 'mongodb://localhost:27017';

let db = null;

MongoClient.connect(url, function(err, client) {
    console.log("Connected successfully to server");
    db = client.db('pets');
});

// read all pets
server.get('/pets', (req, res) => {
    db.collection('pets').find({}).toArray(function(err, docs) {
        res.send(docs);
    });
});

// read one pet information using id
server.get('/pets/:id', (req, res) => {
    console.log('req.params.id', req.params.id);
    db.collection('pets').findOne({_id: parseInt(req.params.id)}, function(err, doc){
        if(err){
            console.log(err);
            res.send(err);
        }
        else{
            console.log(doc);
            res.send(doc);
            
        }
    });
});

// insert a pet
server.put('/pets', (req, res) => {
    console.log('body', req.body);
    db.collection('pets').insertOne({_id: req.body.id,name:req.body.name,age:req.body.age});
    res.send("inserted successfully");
});


// update a pet
server.post('/pets/:id', (req, res) => {
    console.log('id', req.body.id);
    db.collection('pets').updateOne({_id: req.body.id},{$set:{name:req.body.name,age:req.body.age}});
    res.send('updated successfully');
});

// delete pet 
server.delete('/pets/:id', (req, res) => {
    console.log('id', req.body.id);
    db.collection('pets').deleteOne({_id: req.body.id});
    res.send('deleted successfully');
});
server.listen(5000);


```

## How to run your code

1. switch to your terminal again or you can open terminal in your VS code itself.
2. Execute the below command

```
node index.js
```

## How to Test

### 1. Get or Read pets information

* Open your browser and hit
  1. to retrieve all pets information

```
http://localhost:5000/pets 
```
  2. to retrieve one pet information

```
http://localhost:5000/pets/2
```

### 2. Insert or PUT pets information

1. Open Postman
2. Select PUT as protocol and `http://localhost:5000/pets` as URL
3. Configure the headers to send JSON data by making key as `Content-Type` and value as `application/json`.
4. Send sample test data in body.

Sample test data:
```
{
	"id": 3,
	"name": "sunny",
	"age": 3
}
```
5. You can see the response in the bottom of the page.

### 3. Update or POST 

1. Open Postman
2. Select POST as protocol and `http://localhost:5000/pets/1` as URL
3. Configure the headers to send JSON data by making key as `Content-Type` and value as `application/json`.
4. Send sample test data in body.

Sample test data:
```
{
	"name": "mypuppy",
	"age": "10"
}
```
5. You can see the response in the bottom of the page as `updated successfully`.


### 4. Delete
1. Open Postman
2. Select DELETE as protocol and `http://localhost:5000/pets/1` as URL and click Send.
3. You can see the response in the bottom of the page as `deleted successfully`.

