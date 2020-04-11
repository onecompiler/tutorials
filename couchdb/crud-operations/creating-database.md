A database is outermost structure which contains documents. You can create a database using curl in the following way.

```sh
curl -X PUT http://127.0.0.1:5984/employee  
```

If the database is succesfully created, you will receive following response
```sh
{
   "ok": true
}
```

If you try to create database with same name, you will receive following response
```sh
{
    "error": "file_exists",
    "reason": "The database could not be created, the file already exists."
}
```

You can display all the databases created in the followin manner
```sh
curl -X GET http://127.0.0.1:5984/_all_dbs
["employee"]
```