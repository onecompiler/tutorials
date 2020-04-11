In this sub section, let us learn the usage of below commands with examples.

## 1. CREATE 
CREATE command is used to create a table, schema or an index.
### Syntax:
```sql
         CREATE TABLE table_name (
                column1 datatype,
                column2 datatype,
                ....) IN DATABASE_NAME.TABLESPACE_NAME;
``` 
### Example:
```sql
        CREATE TABLE CUSTOMERS(
            InsuranceID INT,
            Name VARCHAR(50),
            DOB  DATE, 
            NIN INT, 
            Location VARCHAR(255)
        ) IN SAMPLEDB.SAMPLETS;
```
## 2. ALTER
 ALTER command is used to add, modify or delete columns or constraints from the database table.
        
###    Syntax: 
```sql 
ALTER TABLE Table_name ADD column_name datatype;
```
###    Example:
```sql
 ALTER TABLE CUSTOMERS ADD email_id VARCHAR(50);
```
## 3. TRUNCATE
 TRUNCATE command is used to delete the data present in the table but this will not delete the table.
###    Syntax: 
```sql
TRUNCATE table table_name [DROP STORAGE/REUSE STORAGE] [IGNORE DELETE TRIGGERS/RESTRICT WHEN DELETE TRIGGERS] [IMMEDIATE];
```
### Example: 
Empty an unused table `CUSTOMERS` regardless of any existing triggers and return its allocated space
```sql
TRUNCATE table CUSTOMERS DROP STORAGE IGNORE DELETE TRIGGERS;
```
## 4. DROP
DROP command is used to delete the table along with its data.

###    Syntax: 
```sql 
DROP TABLE table_name;
```
### Example: 
```sql 
DROP TABLE CUSTOMERS;
```
## 5. RENAME 
RENAME command is used to rename the table name.

###    Syntax:  
```sql
RENAME TABLE table_name to new_table_name; 
```
### Example: 
```sql
RENAME TABLE CUSTOMERS to CUSTOMERINFO;
```
## 6. COMMENT

###  Single-Line Comments: 
Statements starting with `--` are treated as single line comments.
 ###   Example:

 ```sql
  --Line1;
  ```

 ###   Bracketed comments: 

 Statements enclosed in `/**/` are treated as bracketed comments

 ```sql
    /* Line1,
    Line2 */
 ````