CREATE command is used to create a table. Here column family is used to store data just like tables in RDBMS.

Syntax

```sh
CREATE (TABLE | COLUMNFAMILY) <tablename>  
('<column-definition>' , '<column-definition>')  
(WITH <option> AND <option>)   
```

or

```sh
CREATE TABLE tablename(  
   column1 name datatype PRIMARYKEY,  
   column2 name data type,  
   column3 name data type.  
   )  

```

Example : 
```sh
CREATE TABLE student(  
   id int PRIMARY KEY,  
   name text,  
   city text,     
   mobile varint  
);
```   
