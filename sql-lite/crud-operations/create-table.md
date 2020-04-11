`CREATE TABLE` query is used to create a table in the database.

```sh
CREATE TABLE table_name(  
   column1 datatype,  
   column2 datatype,  
   column3 datatype,  
   .....  
   columnN datatype,  
);  
```
#### Example

```sh
CREATE TABLE STUDENT(  
   ID INT PRIMARY KEY     NOT NULL,  
   NAME           TEXT    NOT NULL,  
   AGE            INT     NOT NULL,  
   TOTAL          INT 
);   

```

This command is used to list the databases created.
```sh
.databases
```

Database files are created in root folder.