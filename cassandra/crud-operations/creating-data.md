INSERT command is used to insert data into columns of the table.

Syntax

```sh
INSERT INTO <tablename>  
(<column1 name>, <column2 name>....)  
VALUES (<value1>, <value2>....)  
USING <option>   
```


Example : 
```sh
INSERT INTO student (id, name, city)   
VALUES(1, 'Paul', 'Texas');  
INSERT INTO student (id, name, city)   
VALUES(2, 'John', 'New York');
INSERT INTO student (id, name, city)   
VALUES(1, 'Max', 'New Jersey');
```   
