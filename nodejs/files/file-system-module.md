File system module allows you to interact with the files present on your computer.

## How to include file system module

```javascript
let fs=require('fs');
```

## CRUD operations on file system

### CREATE

You can create files by using below methods
* fs.appendFile()
* fs.open()
* fs.writeFile()


### READ

`fs.readFile()` is used to read a file present in your computer.

### UPDATE

You can update files by using below methods
* fs.appendFile()
* fs.writeFile()

### DELETE

`fs.unlink()` is used to delete a specific file present in your computer.



## Code

```javascript
const express = require('express');
const server = express();
var fs = require('fs');

const bodyParser = require('body-parser');
server.use(bodyParser.urlencoded({
    limit: "4mb",
    extended: false
}));
server.use(bodyParser.json({ limit: "4mb" }));

//create
server.put('/',(req,res) => {
    let content = req.body.content;
    let filename = req.body.filename;
    fs.writeFile(filename, content, function (err) {
        if (err){
            res.send("err:" + err);  
        }
        res.send('File created');
    });
});

//read
server.get('/', (req, res) => {
    fs.readFile('welcome.txt', function(err, doc) {
        if(err){
            res.send("err:" + err);
        }
        if(doc){
            res.send(doc);
        }
    });
});

// update
server.post('/',(req,res) => {
    let content = req.body.content;
    let filename = req.body.filename;
    fs.appendFile(filename, content, function (err) {
        if (err){
            res.send("err:" + err);  
        }
        res.send('File updated');
    });
});

//delete
server.delete('/',(req,res)=>{
    fs.unlink('welcome.txt', function (err) {
        if (err){
            res.send("err:" + err);  
        }
        res.send('File deleted!');
    });
});

server.listen(3000,()=>console.log('server started on 3000'));
```

## How to Test

## 1. CREATE

1. Open Postman
2. Select `PUT` as protocol and `http://localhost:3000/` as URL
3. Configure the headers to send JSON data by making key as `Content-Type` and value as `application/json`.
4. Send sample test data in body.

Sample test data:
```
{
"filename" : "welcome.txt",
"content" : "Happy Learning!!!"
}
```
5. You can see the response as `file created` in the bottom of the page.

## 2. READ
1. Open your browser and hit `http://localhost:3000/`

[or]

2. Go to Postman application and hit `http://localhost:3000/` after selecting `GET` as the protocol.

## 3. UPDATE


1. Open Postman
2. Select `POST` as protocol and `http://localhost:3000/` as URL
3. Configure the headers to send JSON data by making key as `Content-Type` and value as `application/json`.
4. Send sample test data in body.

Sample test data:
```
{
"filename" : "welcome.txt",
"content" : "Hey!! Happy Learning!!!"
}
```
5. You can see the response as `file updated` in the bottom of the page.

### 4. DELETE

1. Open Postman
2. Select DELETE as protocol and `http://localhost:3000/` as URL and click Send.
3. You can see the response in the bottom of the page as `file deleted`.


