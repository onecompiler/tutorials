A keyspace is RDBMS database which contains tables, columns and data.

### Syntax
```sh
CREATE KEYSPACE <identifier> WITH <properties>   
```

```sh
Create keyspace KeyspaceName with replicaton={'class':strategy name,   
'replication_factor': No of replications on different nodes}   
```

### Strategy, Replication Factor

There are two types of strategy declarations.

* Simple Strategy - It is used in case of single data center. This is not a wise choice for production, it may lead to latency.
* NetworkTopologyStrategy - It is used in case of more data centers. Replication factor is provided for each data center separately.

Replication Factor- Replication factor is the number of replicas of data placed on different nodes. Atleast 3 replication factor is required to attain no single point of failure.

Example

```sh
CREATE KEYSPACE whichdb  
WITH replication = {'class':'SimpleStrategy', 'replication_factor' : 3};  
 ```


 DESCRIBE Keyword is used to display all the keyspaces created.
```sh
DESCRIBE keyspaces

system_schema system system_traces
system_auth system_distributed whichdb
 ```

USE keyword is used to use the keyspace
```sh
USE whichdb
```