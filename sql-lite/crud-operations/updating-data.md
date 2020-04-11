`UPDATE` query is used to update the existing data in the table. It is used with where clause otherwise all the rows will be updated 

```sh
UPDATE table_name  
SET column1 = value1, column2 = value2...., columnN = valueN  
WHERE [condition];   
```

#### Example

```sh
UPDATE STUDENT SET AGE = AGE + 2 WHERE ID = 5;   

```