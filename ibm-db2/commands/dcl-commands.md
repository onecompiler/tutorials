In this sub section, let us learn the usage of below commands in detail.

# 1. GRANT

GRANT statement is used to provide access privileges to users to access the database.

## GRANT PRIVILEGES ON DATABASE

### Syntax:

```sql
GRANT privileges ON DATABASE Database_name TO user/role rolename/PUBLIC [WITH GRANT OPTION];
```

Let us understand the privileges briefly

|Privileges|Description|
|--------|---------|
|DBADM| Grants database administrator authority|
|DBCTRL| Grants database control authority|
|DBMAINT| Grants database maintenance authority|
|CREATETAB| Grants the privilege to create new tables|
|CREATETS| Grants the privilege to create new table spaces|
|DISPLAYDB| Grants the privilege to issue the DISPLAY DATABASE command|
|DROP|Grants the privilege to issue the DROP or ALTER DATABASE statements|
|IMAGCOPY|Grants the privilege to run the COPY, MERGECOPY, and QUIESCE utilities and also to run MODIFY RECOVERY utility|
|LOAD|Grants the privilege to use the LOAD utility to load tables|
|RECOVERDB|Grants the privilege to use the RECOVER and REPORT utilities to recover table spaces and indexes|
|REORG|Grants the privilege to use the REORG utility to reorganize table spaces and indexes|
|REPAIR|Grants the privilege to use the REPAIR and DIAGNOSE utilities|
|STARTDB|Grants the privilege to issue the START DATABASE command|
|STATS|Grants the privilege to use the RUNSTATS utility to update statistics|
|STOPDB| Grants the privilege to issue the STOP DATABASE command|

### Example

With this command, all the local users can execute the DISPLAY database command

```sql
   GRANT DISPLAYDB ON DATABASE DSN8D143A  TO PUBLIC;
```
## GRANT PRIVILEGES ON Tables and Views

### Syntax

```sql
GRANT privileges ON TABLE Table_name TO user/role rolename/PUBLIC [WITH GRANT OPTION];
```
Let us understand the privileges briefly

|Privileges|Description|
|--------|---------|
|ALL or ALL PRIVILEGES|Grants all table or view privileges|
|ALTER|Grants the privilege to alter the specified table|
|DELETE|Grants the privilege to delete rows in the specified table or view|
|INDEX|Grants the privilege to create an index on the specified table|
|INSERT|Grants the privilege to insert rows into the specified table or view|
|REFERENCES (Column_name)|Grants the privilege to add or drop a referential constraint in which the specified table is a parent using only those columns that are specified in the column list as a parent key|
|SELECT|Grants the privilege to create a view or read data from the specified table or view|
|TRIGGER|Grants the privilege to create a trigger on the specified table
|UPDATE|Grants the privilege to update rows in the specified table or view. |



## 2. REVOKE

 REVOKE statement is used to withdraw the access priviliges given to a user by GRANT statement.

## REVOKE PRIVILEGES ON DATABASE

### Syntax:

```sql
REVOKE privileges ON DATABASE Database_name FROM user/role rolename/PUBLIC BY user/role rolename/ALL;
```

Let us understand the privileges briefly

|Privileges|Description|
|--------|---------|
|DBADM| Revokes the database administrator authority|
|DBCTRL| Revokes the database control authority|
|DBMAINT| Revokes the database maintenance authority|
|CREATETAB| Revokes the  privilege to create new tables|
|CREATETS| Revokes the  privilege to create new table spaces|
|DISPLAYDB| Revokes the the privilege to issue the DISPLAY DATABASE command|
|DROP|Revokes the the privilege to issue the DROP or ALTER DATABASE statements|
|IMAGCOPY|Revokes the the privilege to run the COPY, MERGECOPY, and QUIESCE utilities and also to run MODIFY RECOVERY utility|
|LOAD|Revokes the privilege to use the LOAD utility to load tables|
|RECOVERDB|Revokes the privilege to use the RECOVER and REPORT utilities to recover table spaces and indexes|
|REORG|Revokes the privilege to use the REORG utility to reorganize table spaces and indexes|
|REPAIR|Revokes the privilege to use the REPAIR and DIAGNOSE utilities|
|STARTDB|Revokes the privilege to issue the START DATABASE command|
|STATS|Revokes the privilege to use the RUNSTATS utility to update statistics|
|STOPDB| Revokes the privilege to issue the STOP DATABASE command|

### Example

With this command,  DISPLAY database command execution authorization will be revoked from all local users.

```sql
   REVOKE DISPLAYDB ON DATABASE DSN8D143A  FROM PUBLIC;
```
## REVOKE PRIVILEGES ON Tables and Views

### Syntax

```sql
REVOKE privileges ON TABLE Table_name FROM user/role rolename/PUBLIC BY user/role rolename/ALL;
```
Let us understand the privileges briefly

|Privileges|Description|
|--------|---------|
|ALL or ALL PRIVILEGES|Revokes all table or view privileges|
|ALTER|Revokes the privilege to alter the specified table|
|DELETE|Revokes the privilege to delete rows in the specified table or view|
|INDEX|Revokes the privilege to create an index on the specified table|
|INSERT|Revokes the privilege to insert rows into the specified table or view|
|REFERENCES (Column_name)|Revokes the privilege to add or drop a referential constraint in which the specified table is a parent using only those columns that are specified in the column list as a parent key|
|SELECT|Revokes the privilege to create a view or read data from the specified table or view|
|TRIGGER|Revokes the privilege to create a trigger on the specified table
|UPDATE|Revokes the privilege to update rows in the specified table or view. |
