## Reading whole table from a keyspace
SELECT is used to read data from cassandra table.

```sh
SELECT FROM <tablename>   

SELECT * FROM student;  
```

## Reading Particular columns in a table
Required columns are specified instead of *.
```sh
SELECT id, name, city FROM student;   
```

## Reading data from table based on condition
WHERE clause is used to return a data based on given condition

```sh
SELECT * FROM student WHERE id=2;    
```


