## Cassandra Query Language
Cassandra Query Language (CQL) is used to access Cassandra database through queries. CQL Treats Database as keyspaces as a container of tables.

## Terms Related to Cassandra
* Node  - Node is place where data is stored.
* Data Center - Data center is a collection of related nodes.
* Cluster - Cluster is component containing one or more data centers.
* Commit log - This is a crash recovery mechanism, every write operation is written into the log.

## Data Replication in Cassandra
In cassandra nodes in a cluster acts as replicas for a given piece of data. If a node responds with out-of-date value, Cassandra will return the recent value and performs a repair in the background to repair the old value.

## Keyspace, Table, Data
Cassandra provides keyspaces(Database) which contains multiple tables. Tables contain data

## Cassandra CQLsh
Cassandra CQL shell is utility to interact with Cassandra using CQL Queries. 
