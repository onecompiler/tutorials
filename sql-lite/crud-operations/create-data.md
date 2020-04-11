`INSERT INTO` query is used to add new rows into table.

### Syntax 1

If you are not inserting all the columns in a row, you need to mention the column name also.

```sh
INSERT INTO TABLE_NAME [(column1, column2, column3,...columnN)]    
VALUES (value1, value2, value3,...valueN);   
```

### Syntax 2

If you are inserting all the columns in a row, you need need mention the column names.

```sh
INSERT INTO TABLE_NAME VALUES (value1,value2,value3,...valueN);   
```

Eg:
```sh
INSERT INTO STUDENT VALUES (1, 'John', 18, 85);   
```