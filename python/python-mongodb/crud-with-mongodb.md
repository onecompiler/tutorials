MongoDB is one of the most popular NoSQL databases.  

You need to install a python driver `pymongo` to connect with MongoDB. 

## How to install pymongo driver

```sh
python -m pip install pymongo
```

## Create Database

consider you are creating a database called "sample".

```py
import pymongo

db = pymongo.MongoClient("mongodb://localhost:27017/")

mydb = db["sample"]
```

## create collection

create a collection named "details".

```py
import pymongo
db = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = db["sample"]

mycln = mydb["details"]
```

## How to insert a document

`insertone()` is used to insert a single document and `insertmany()` is used to insert multiple documents.


```py
import pymongo
db = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = db["sample"]
mycln = mydb["details"]
mydict = { "name": "foo", "age": 20 }

#insert a single document
doc = mycln.insert_one(mydict)

# insert multiple documents
mylist =[
    {"name": "foo", "age": 20},
    {"name": "bar", "age": 25},
    {"name": "apple", "age": 30}
]
doc1 = mycln.insert_many(mylist)

# Mongodb by default creates an id for each document
print(doc1.inserted_ids)

```

## How to read the documents

`find()` and `find_one()` are used to query the database

```py
import pymongo
db = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = db["sample"]
mycln = mydb["details"]

#to return all the documents
for doc in mycln.find():
  print(doc)

# to return first occurence
doc1=mycln.find_one()
print(doc1)

#to return list of names whose age is greater than 20
qry = { "age": { "$gt": 30 } }

for doc3 in mycln.find(qry)
print(doc3)
```

## How to update a document

`update_one` and `update_many` are used to update the document in Mongodb.

```py
import pymongo
db = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = db["sample"]
mycln = mydb["details"]

# to update a single document
mycln.update_one({"name" : "foo"}, {"$set":{"age": 23}})

doc1 = mycln.find({"name" : "foo"})
print(doc1)
```

## How to delete a document
`delete_one()` and `delete_many()` are used to delete documents in Mongodb.


```py
import pymongo
db = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = db["sample"]
mycln = mydb["details"]

# to delete a single document

doc1 = mycln.delete_one({"name" : "foo"})
print(doc1)
```


