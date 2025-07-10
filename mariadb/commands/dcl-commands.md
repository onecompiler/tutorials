In this sub section, let us learn the usage of DCL (Data Control Language) commands in MariaDB.

# 1. CREATE USER
CREATE USER statement is used to create new MariaDB user accounts.

## Syntax:
```sql
CREATE USER [IF NOT EXISTS] 'username'@'host' 
IDENTIFIED BY 'password'
[WITH resource_options];
```

### Examples:

#### Create user with access from localhost only:
```sql
CREATE USER 'john'@'localhost' IDENTIFIED BY 'secure_password123';
```

#### Create user with access from any host:
```sql
CREATE USER 'jane'@'%' IDENTIFIED BY 'another_secure_pass';
```

#### Create user with specific IP access:
```sql
CREATE USER 'admin'@'192.168.1.100' IDENTIFIED BY 'admin_pass';
```

#### Create user with resource limits:
```sql
CREATE USER 'limited_user'@'localhost' IDENTIFIED BY 'pass123'
WITH MAX_QUERIES_PER_HOUR 100
     MAX_CONNECTIONS_PER_HOUR 50
     MAX_USER_CONNECTIONS 5;
```

# 2. GRANT
GRANT statement is used to provide access privileges to users and roles in MariaDB.

## Syntax:
```sql
GRANT privilege_type [(column_list)]
ON [object_type] privilege_level
TO user_or_role [, user_or_role] ...
[WITH GRANT OPTION];
```

## MariaDB Privilege Types:

### Global Privileges:
- **ALL PRIVILEGES**: All privileges except GRANT OPTION
- **CREATE**: Create databases and tables
- **DROP**: Drop databases and tables
- **DELETE**: Delete rows from tables
- **INSERT**: Insert rows into tables
- **SELECT**: Read data from tables
- **UPDATE**: Update data in tables
- **PROCESS**: View processes via SHOW PROCESSLIST
- **RELOAD**: Execute FLUSH commands
- **SHUTDOWN**: Shutdown MariaDB server
- **FILE**: Read/write files on server
- **REFERENCES**: Create foreign keys
- **INDEX**: Create and drop indexes
- **ALTER**: Alter table structure
- **SHOW DATABASES**: See all databases
- **SUPER**: Administrative operations
- **CREATE TEMPORARY TABLES**: Create temporary tables
- **LOCK TABLES**: Lock tables
- **EXECUTE**: Execute stored procedures
- **REPLICATION SLAVE**: Read binary logs
- **REPLICATION CLIENT**: Get replication status
- **CREATE VIEW**: Create views
- **SHOW VIEW**: See view definitions
- **CREATE ROUTINE**: Create stored procedures/functions
- **ALTER ROUTINE**: Alter stored procedures/functions
- **CREATE USER**: Create users
- **EVENT**: Create events
- **TRIGGER**: Create triggers

### MariaDB-Specific Privileges:
- **DELETE HISTORY**: Delete historical data (system versioned tables)
- **BINLOG ADMIN**: Binary log administration
- **BINLOG MONITOR**: Monitor binary logs
- **BINLOG REPLAY**: Replay binary logs
- **CONNECTION ADMIN**: Connection administration
- **FEDERATED ADMIN**: Federated tables administration
- **READ_ONLY ADMIN**: Override read_only setting
- **REPLICATION MASTER ADMIN**: Master replication administration
- **REPLICATION SLAVE ADMIN**: Slave replication administration
- **SET USER**: Set user for current session

### Examples:

#### Grant all privileges on a database:
```sql
GRANT ALL PRIVILEGES ON company_db.* TO 'john'@'localhost';
```

#### Grant specific privileges on a table:
```sql
GRANT SELECT, INSERT, UPDATE ON company_db.employees TO 'hr_user'@'localhost';
```

#### Grant column-level privileges:
```sql
GRANT SELECT (employee_id, first_name, last_name), 
      UPDATE (email, phone) 
ON company_db.employees 
TO 'limited_user'@'localhost';
```

#### Grant with GRANT OPTION:
```sql
GRANT SELECT, INSERT ON sales_db.* TO 'manager'@'%' WITH GRANT OPTION;
```

#### Grant execute privilege on stored procedure:
```sql
GRANT EXECUTE ON PROCEDURE company_db.calculate_bonus TO 'payroll'@'localhost';
```

#### Grant privileges on all databases:
```sql
GRANT SELECT, INSERT ON *.* TO 'analyst'@'localhost';
```

# 3. ROLES (MariaDB 10.0.5+)
Roles are named collections of privileges that can be granted to users.

## Creating Roles:
```sql
CREATE ROLE [IF NOT EXISTS] role_name;
```

### Examples:

#### Create roles:
```sql
CREATE ROLE developer;
CREATE ROLE tester;
CREATE ROLE db_admin;
CREATE ROLE read_only_user;
```

#### Grant privileges to roles:
```sql
-- Developer role
GRANT SELECT, INSERT, UPDATE, DELETE ON app_db.* TO developer;
GRANT CREATE, ALTER, DROP ON app_db.* TO developer;
GRANT EXECUTE ON app_db.* TO developer;

-- Tester role
GRANT SELECT ON app_db.* TO tester;
GRANT INSERT, UPDATE, DELETE ON app_db.test_data TO tester;

-- Database admin role
GRANT ALL PRIVILEGES ON *.* TO db_admin WITH GRANT OPTION;

-- Read-only role
GRANT SELECT ON company_db.* TO read_only_user;
```

#### Grant roles to users:
```sql
GRANT developer TO 'alice'@'localhost';
GRANT tester TO 'bob'@'localhost';
GRANT db_admin TO 'charlie'@'localhost';
```

#### Set default role:
```sql
SET DEFAULT ROLE developer FOR 'alice'@'localhost';
```

#### Enable roles in current session:
```sql
SET ROLE developer;
SET ROLE ALL;  -- Enable all granted roles
SET ROLE NONE; -- Disable all roles
```

# 4. REVOKE
REVOKE statement is used to withdraw access privileges from users and roles.

## Syntax:
```sql
REVOKE privilege_type [(column_list)]
ON [object_type] privilege_level
FROM user_or_role [, user_or_role] ...;
```

### Examples:

#### Revoke all privileges:
```sql
REVOKE ALL PRIVILEGES, GRANT OPTION FROM 'john'@'localhost';
```

#### Revoke specific privileges:
```sql
REVOKE DELETE, UPDATE ON company_db.employees FROM 'hr_user'@'localhost';
```

#### Revoke column-level privileges:
```sql
REVOKE SELECT (salary, ssn) ON company_db.employees FROM 'limited_user'@'localhost';
```

#### Revoke role from user:
```sql
REVOKE developer FROM 'alice'@'localhost';
```

#### Revoke privileges from role:
```sql
REVOKE DELETE ON app_db.* FROM tester;
```

# 5. SHOW GRANTS
Display privileges for users and roles.

### Examples:
```sql
-- Show current user's privileges
SHOW GRANTS;

-- Show specific user's privileges
SHOW GRANTS FOR 'john'@'localhost';

-- Show role privileges
SHOW GRANTS FOR developer;

-- Show grants using a role
SHOW GRANTS FOR 'alice'@'localhost' USING developer;
```

# 6. ALTER USER
Modify user accounts.

## Syntax:
```sql
ALTER USER [IF EXISTS] 'username'@'host' 
[IDENTIFIED BY 'new_password']
[WITH resource_options];
```

### Examples:

#### Change password:
```sql
ALTER USER 'john'@'localhost' IDENTIFIED BY 'new_secure_password';
```

#### Set password expiration:
```sql
ALTER USER 'john'@'localhost' PASSWORD EXPIRE;
ALTER USER 'john'@'localhost' PASSWORD EXPIRE INTERVAL 90 DAY;
ALTER USER 'john'@'localhost' PASSWORD EXPIRE NEVER;
```

#### Account locking:
```sql
ALTER USER 'john'@'localhost' ACCOUNT LOCK;
ALTER USER 'john'@'localhost' ACCOUNT UNLOCK;
```

#### Modify resource limits:
```sql
ALTER USER 'limited_user'@'localhost' 
WITH MAX_QUERIES_PER_HOUR 200
     MAX_USER_CONNECTIONS 10;
```

# 7. DROP USER
Remove user accounts.

## Syntax:
```sql
DROP USER [IF EXISTS] 'username'@'host' [, 'username'@'host'] ...;
```

### Examples:
```sql
DROP USER 'john'@'localhost';
DROP USER IF EXISTS 'jane'@'%', 'bob'@'192.168.1.100';
```

# 8. DROP ROLE
Remove roles.

## Syntax:
```sql
DROP ROLE [IF EXISTS] role_name [, role_name] ...;
```

### Examples:
```sql
DROP ROLE developer;
DROP ROLE IF EXISTS tester, read_only_user;
```

# 9. FLUSH PRIVILEGES
Reload privilege tables from disk.

```sql
FLUSH PRIVILEGES;
```

**Note:** Usually not needed after GRANT/REVOKE commands as they update privilege tables automatically. Required when modifying privilege tables directly.

# 10. Best Practices

1. **Use specific hosts**: Avoid using '%' for production users when possible
2. **Principle of least privilege**: Grant only necessary privileges
3. **Use roles**: Group related privileges into roles for easier management
4. **Strong passwords**: Always use strong, unique passwords
5. **Regular audits**: Periodically review user privileges
6. **Remove unused accounts**: Drop accounts that are no longer needed

### Security Example:
```sql
-- Create application-specific user with minimal privileges
CREATE USER 'webapp'@'10.0.0.%' IDENTIFIED BY 'strong_random_password';
GRANT SELECT, INSERT, UPDATE ON myapp.* TO 'webapp'@'10.0.0.%';
GRANT DELETE ON myapp.sessions TO 'webapp'@'10.0.0.%';

-- Create read-only reporting user
CREATE USER 'reporter'@'localhost' IDENTIFIED BY 'another_strong_pass';
GRANT SELECT ON myapp.* TO 'reporter'@'localhost';

-- Create admin role for developers
CREATE ROLE app_developer;
GRANT ALL ON myapp.* TO app_developer;
GRANT app_developer TO 'dev1'@'localhost', 'dev2'@'localhost';
SET DEFAULT ROLE app_developer FOR 'dev1'@'localhost', 'dev2'@'localhost';
```
