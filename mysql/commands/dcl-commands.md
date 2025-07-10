In this sub section, let us learn the usage of MySQL DCL (Data Control Language) commands in detail.

# 1. CREATE USER
The CREATE USER statement is used to create new MySQL user accounts.

## Syntax:
```sql
CREATE USER 'username'@'host' IDENTIFIED BY 'password';
```

### Examples:
```sql
-- Create user with access from localhost only
CREATE USER 'john'@'localhost' IDENTIFIED BY 'SecurePass123!';

-- Create user with access from any host
CREATE USER 'sarah'@'%' IDENTIFIED BY 'MyPassword456!';

-- Create user with access from specific IP
CREATE USER 'admin'@'192.168.1.100' IDENTIFIED BY 'AdminPass789!';

-- Create user without password (not recommended)
CREATE USER 'testuser'@'localhost';
```

# 2. SET PASSWORD
The SET PASSWORD statement is used to change user passwords.

## Syntax:
```sql
SET PASSWORD FOR 'username'@'host' = 'new_password';
```

### Examples:
```sql
-- Change password for specific user
SET PASSWORD FOR 'john'@'localhost' = 'NewSecurePass123!';

-- Change current user's password
SET PASSWORD = 'MyNewPassword456!';

-- Using ALTER USER (MySQL 5.7.6+)
ALTER USER 'john'@'localhost' IDENTIFIED BY 'AnotherPass789!';
```

# 3. GRANT
GRANT statement is used to provide access privileges to users on databases and tables.

## Syntax:
```sql
GRANT privilege_type ON database.table TO 'username'@'host';
```

### MySQL Privilege Types:
- **ALL PRIVILEGES**: All privileges except GRANT OPTION
- **SELECT**: Read data from tables
- **INSERT**: Insert rows into tables
- **UPDATE**: Modify existing data
- **DELETE**: Delete rows from tables
- **CREATE**: Create databases/tables
- **DROP**: Drop databases/tables
- **ALTER**: Modify table structure
- **INDEX**: Create/drop indexes
- **EXECUTE**: Execute stored procedures
- **CREATE VIEW**: Create views
- **SHOW VIEW**: See view definitions
- **CREATE ROUTINE**: Create stored procedures/functions
- **ALTER ROUTINE**: Modify stored procedures/functions
- **TRIGGER**: Create/drop triggers
- **EVENT**: Create/drop/alter events
- **CREATE USER**: Create new users
- **PROCESS**: Show processlist
- **RELOAD**: Execute FLUSH commands
- **REPLICATION SLAVE**: Read binary logs for replication
- **REPLICATION CLIENT**: Get replication status
- **SHOW DATABASES**: See all databases
- **SHUTDOWN**: Shutdown MySQL server
- **SUPER**: Administrative operations
- **GRANT OPTION**: Grant privileges to other users

### Examples:

#### Grant all privileges on a specific database:
```sql
GRANT ALL PRIVILEGES ON mydb.* TO 'john'@'localhost';
```

#### Grant specific privileges on a table:
```sql
GRANT SELECT, INSERT, UPDATE ON mydb.customers TO 'sarah'@'%';
```

#### Grant privileges on all databases:
```sql
GRANT ALL PRIVILEGES ON *.* TO 'admin'@'localhost';
```

#### Grant with GRANT OPTION:
```sql
GRANT SELECT, INSERT ON mydb.* TO 'manager'@'localhost' WITH GRANT OPTION;
```

#### Grant column-specific privileges:
```sql
GRANT SELECT (name, email), UPDATE (email) ON mydb.users TO 'support'@'localhost';
```

#### Grant stored procedure execution:
```sql
GRANT EXECUTE ON PROCEDURE mydb.calculate_salary TO 'hr'@'localhost';
```

#### Grant create and drop privileges:
```sql
GRANT CREATE, DROP ON mydb.* TO 'developer'@'localhost';
```

#### Grant replication privileges:
```sql
GRANT REPLICATION SLAVE ON *.* TO 'replica'@'192.168.1.200';
```

# 4. REVOKE
REVOKE statement is used to withdraw access privileges from users.

## Syntax:
```sql
REVOKE privilege_type ON database.table FROM 'username'@'host';
```

### Examples:

#### Revoke all privileges:
```sql
REVOKE ALL PRIVILEGES ON mydb.* FROM 'john'@'localhost';
```

#### Revoke specific privileges:
```sql
REVOKE DELETE, UPDATE ON mydb.orders FROM 'sarah'@'%';
```

#### Revoke GRANT OPTION:
```sql
REVOKE GRANT OPTION ON mydb.* FROM 'manager'@'localhost';
```

#### Revoke column-specific privileges:
```sql
REVOKE UPDATE (salary, bonus) ON mydb.employees FROM 'hr'@'localhost';
```

#### Revoke all privileges on all databases:
```sql
REVOKE ALL PRIVILEGES, GRANT OPTION FROM 'testuser'@'localhost';
```

# 5. SHOW GRANTS
The SHOW GRANTS statement displays the privileges assigned to a user.

## Syntax:
```sql
SHOW GRANTS FOR 'username'@'host';
```

### Examples:
```sql
-- Show grants for specific user
SHOW GRANTS FOR 'john'@'localhost';

-- Show grants for current user
SHOW GRANTS;
SHOW GRANTS FOR CURRENT_USER;
SHOW GRANTS FOR CURRENT_USER();

-- Example output:
-- GRANT USAGE ON *.* TO 'john'@'localhost'
-- GRANT SELECT, INSERT, UPDATE ON `mydb`.* TO 'john'@'localhost'
```

# 6. DROP USER
The DROP USER statement removes one or more MySQL user accounts.

## Syntax:
```sql
DROP USER 'username'@'host';
```

### Examples:
```sql
-- Drop single user
DROP USER 'testuser'@'localhost';

-- Drop multiple users
DROP USER 'user1'@'localhost', 'user2'@'%';

-- Drop if exists (MySQL 5.7+)
DROP USER IF EXISTS 'olduser'@'localhost';
```

# 7. FLUSH PRIVILEGES
FLUSH PRIVILEGES reloads the grant tables in the mysql database.

## Syntax:
```sql
FLUSH PRIVILEGES;
```

**Note:** Usually not needed when using GRANT, REVOKE, CREATE USER, DROP USER, but required when modifying grant tables directly.

# 8. Additional Examples

### Create a read-only user:
```sql
CREATE USER 'readonly'@'localhost' IDENTIFIED BY 'ReadOnly123!';
GRANT SELECT ON mydb.* TO 'readonly'@'localhost';
```

### Create an application user with limited privileges:
```sql
CREATE USER 'app_user'@'localhost' IDENTIFIED BY 'AppPass456!';
GRANT SELECT, INSERT, UPDATE, DELETE ON mydb.* TO 'app_user'@'localhost';
```

### Create a backup user:
```sql
CREATE USER 'backup'@'localhost' IDENTIFIED BY 'BackupPass789!';
GRANT SELECT, SHOW VIEW, TRIGGER, LOCK TABLES, PROCESS ON *.* TO 'backup'@'localhost';
```

### Create a developer with full database privileges:
```sql
CREATE USER 'dev'@'localhost' IDENTIFIED BY 'DevPass123!';
GRANT ALL PRIVILEGES ON dev_db.* TO 'dev'@'localhost';
GRANT CREATE, DROP ON *.* TO 'dev'@'localhost';
```

### Rename a user (MySQL 5.7.6+):
```sql
RENAME USER 'old_name'@'localhost' TO 'new_name'@'localhost';
```

### Lock/Unlock user account (MySQL 5.7.6+):
```sql
-- Lock account
ALTER USER 'john'@'localhost' ACCOUNT LOCK;

-- Unlock account
ALTER USER 'john'@'localhost' ACCOUNT UNLOCK;
```

### Set password expiration:
```sql
-- Password expires in 90 days
ALTER USER 'john'@'localhost' PASSWORD EXPIRE INTERVAL 90 DAY;

-- Password never expires
ALTER USER 'john'@'localhost' PASSWORD EXPIRE NEVER;

-- Force password change on next login
ALTER USER 'john'@'localhost' PASSWORD EXPIRE;
```

# Best Practices:
1. Always use strong passwords for user accounts
2. Grant only the minimum privileges required
3. Use specific hostnames instead of '%' when possible
4. Regularly audit user privileges with SHOW GRANTS
5. Remove unused user accounts
6. Use separate accounts for different applications
7. Avoid granting SUPER or ALL PRIVILEGES unless absolutely necessary
8. Use GRANT OPTION sparingly
9. Document user privileges and their purposes
10. Regularly review and update user permissions