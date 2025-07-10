# Introduction to Cassandra

Apache Cassandra is a free and open-source, distributed NoSQL database management system designed to handle large amounts of data across many commodity servers, providing high availability with no single point of failure. It offers robust support for clusters spanning multiple datacenters, with asynchronous masterless replication allowing low latency operations for all clients.

## Key Features

- **Distributed**: Every node in the cluster has the same role. There is no single point of failure
- **Scalability**: Designed to scale horizontally by simply adding more nodes
- **Fault-tolerant**: Data is automatically replicated to multiple nodes
- **High Performance**: Designed for fast write and read operations
- **Flexible Schema**: Supports structured, semi-structured, and unstructured data
- **Eventually Consistent**: Uses eventual consistency to achieve high availability

## When to Use Cassandra

Cassandra is ideal for:
- Applications that need to store and retrieve large amounts of data
- Time-series data (IoT sensors, logs, events)
- Real-time analytics and reporting
- Applications requiring high write throughput
- Geographically distributed applications
- Applications that cannot afford any downtime

## Cassandra Query Language (CQL)

Cassandra Query Language (CQL) is the primary interface for communicating with Cassandra. CQL is similar to SQL but designed for Cassandra's distributed architecture. It treats the database as a container of keyspaces, which in turn contain tables.

## Core Concepts

### Architecture Components

- **Node**: A single server running Cassandra where data is stored
- **Rack**: A logical grouping of nodes, typically representing a physical rack or availability zone
- **Data Center**: A collection of related nodes, typically in the same physical location
- **Cluster**: The complete set of nodes that store your data across one or more data centers
- **Commit Log**: A crash-recovery mechanism where every write is first written to ensure durability
- **Memtable**: An in-memory data structure where data is written after the commit log
- **SSTable**: Sorted String Table - immutable data files where memtables are periodically flushed
- **Compaction**: Process of merging SSTables to optimize read performance and reclaim disk space

## Data Replication

Cassandra stores replicas on multiple nodes to ensure reliability and fault tolerance. Key concepts:

- **Replication Factor**: Number of copies of data maintained across the cluster
- **Replication Strategy**: Algorithm for determining which nodes store replicas
  - **SimpleStrategy**: Used for single data center deployments
  - **NetworkTopologyStrategy**: Used for multi-data center deployments
- **Consistency Level**: Number of replicas that must acknowledge a read/write operation
- **Hinted Handoff**: Temporary storage of writes when a replica node is down
- **Read Repair**: Background process that ensures data consistency across replicas

## Data Model

### Keyspace
A keyspace is the top-level container in Cassandra, similar to a database in relational systems. It defines:
- Replication strategy
- Replication factor
- Data center configuration

### Table
Tables in Cassandra contain:
- **Columns**: Named values with specific data types
- **Rows**: Identified by a primary key
- **Primary Key**: Consists of partition key and optional clustering columns
- **Partition Key**: Determines which node stores the data
- **Clustering Columns**: Sort data within a partition

## CQL Shell (cqlsh)

The Cassandra Query Language Shell (cqlsh) is a command-line interface for interacting with Cassandra using CQL commands. It provides:
- Interactive CQL command execution
- Script execution from files
- Data import/export capabilities
- Schema exploration commands

To start cqlsh after installation:
```bash
cqlsh
```

Or connect to a specific host:
```bash
cqlsh <hostname> <port>
``` 

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

### Installing From Debian Packages
```sh
echo "deb http://www.apache.org/dist/cassandra/debian 311x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list
sudo apt-get update
sudo apt-get install cassandra
```

### Installing From RPM Packages
```sh
[cassandra]
name=Apache Cassandra
baseurl=https://www.apache.org/dist/cassandra/redhat/311x/
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://www.apache.org/dist/cassandra/KEYS

sudo yum install cassandra
```
