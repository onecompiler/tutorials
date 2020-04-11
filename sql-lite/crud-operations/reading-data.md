## Reading all documents from a collection
`SELECT` is used to read fetch data from a table in database.

## SELECT All the data in table

```sh
SELECT * FROM table_name;   
```

## SELECT Required columns in a table

```sh
SELECT column1, column2, columnN FROM table_name;   
```

## SELECT based on condition

```sh
SELECT column1, column2, columnN   
FROM table_name  
WHERE [condition]  
```

#### Example

```sh
SELECT * FROM STUDENT WHERE ID >= 10 AND TOTAL >= 90;   
```