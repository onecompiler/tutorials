In this sub section, let us learn the usage of below commands in detail.

# 1. GRANT
GRANT statement is used to provide access privileges to users to access the database.

### Syntax:
```sql
GRANT privileges_type ON object_type TO user;
```

**Note:** Privileges_type can be SELECT, INSERT, UPDATE, DELETE, TRUNCATE, REFERENCES, TRIGGER, CREATE, ALL. You can also specify combination of these privileges in a statement.
object_type is table/function/procedure.



### Grant global privileges to user

```sql
GRANT ALL ON *.* TO 'username'@'hostname';
GRANT SELECT, INSERT ON *.* TO 'username'@'hostname';

```

### Grant database privileges to a user:
```sql
GRANT ALL ON dbname.* TO 'username'@'hostname';
GRANT SELECT, INSERT ON dbname.* TO 'username'@'hostname';
```


### Grant table privileges to a user
```sql
GRANT ALL ON dbname.tblname TO 'username'@'hostname';
GRANT SELECT, INSERT ON dbname.tblname TO 'username'@'hostname'';
```
### To Specify server to permit only encrypted connections
```sql
GRANT ALL PRIVILEGES ON test.* TO 'root'@'localhost' REQUIRE SSL;
```

## 2. REVOKE
 REVOKE statement is used to withdraw the access priviliges given to a user by GRANT statement.

#### Syntax:

```sql
REVOKE privileges ON object FROM user;
```

#### Example:
```sql
 REVOKE DELETE, UPDATE ON ORDERS FROM customer1;
 ```
