Cassandra is a free and open-source, distributed NoSQL database management system from Apache. It is designed to handle huge amount of data providing high performance, high scalability across servers without any failure.

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

# Installation

## Installing on MacOS

Easiest way to get Cassandra installed on MacOS is via Homebrew. You can run the following command to install Cassandra in MacOS
```sh
brew install cassandra
```

## Installing on Windows

We can download the latest version of cassandra from the following website
http://cassandra.apache.org/download/

Run the installation and open cassandra

### Installing Fron Debian Packages
```sh
echo "deb http://www.apache.org/dist/cassandra/debian 311x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list
sudo apt-get update
sudo apt-get install cassandra
```

### Installing Fron RPM Packages
```sh
[cassandra]
name=Apache Cassandra
baseurl=https://www.apache.org/dist/cassandra/redhat/311x/
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://www.apache.org/dist/cassandra/KEYS

sudo yum install cassandra
```
