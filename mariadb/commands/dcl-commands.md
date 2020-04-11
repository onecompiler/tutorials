In this sub section, let us learn the usage of below commands in detail.

# 1. GRANT
GRANT statement is used to provide access privileges to users to access the database.

## Syntax:
```sql
GRANT privileges ON object TO user;
```

**Note:** Privileges can be SELECT, INSERT, UPDATE, DELETE, TRUNCATE, REFERENCES, TRIGGER, CREATE, ALL. You can also specify combination of these privileges in a statement.


### GRANT User only to access only from localhost

```sql
GRANT USAGE ON *.* TO 'username'@localhost IDENTIFIED BY 'password';
```

### GRANT User only to access from any other computer on the network

```sql
GRANT USAGE ON *.* TO 'user'@'%' IDENTIFIED BY 'password';
```

### Grant all privileges to a user to a specific database: 

```sql
GRANT ALL privileges ON `dbname`.* TO 'username';
```


### Grant permission to create database:
```sql
ALTER USER username CREATEDB;
```


### Grant superuser access to a user
```sql
ALTER USER myuser WITH SUPERUSER;
```


## 2. REVOKE
 REVOKE statement is used to withdraw the access priviliges given to a user by GRANT statement.

### Syntax:

```sql
REVOKE privileges ON object FROM user;
```

### Example:
```sql
 REVOKE DELETE, UPDATE ON ORDERS FROM customer1;
 ```
