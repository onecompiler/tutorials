## Clusters

Cluster is a logical connection of three or more queue managers. Care should be taken in planning and configuring clusters to maximize its benefits. 

## Key Benefits

1. Work load balancing
    Clusters allows you to define same instance of the queue in multiple queue managers and hence availability of the queue will be more and the work can be distributed across multiple instances.

2. Lower system administration

    You dont need to create multiple MQ objects like remote queues, transmission queues and channels to have the connectivity between queue managers and hence administration effort will be lowered.        

## How to setup a cluster

1. Determine which queue manager or queue managers(preferably two) can be full repositories and alter the QM definition.

```
ALTER QMGR REPOS(cluster_name)
```

2. Every queue manager in the cluster must have cluster receiver channel pointing to itself.

```
DEFINE CHANNEL(channel_name) CHLTYPE(CLUSRCVR) TRTYPE(TCP) CONNAME('my_host_name_or_ip_address(port)') CLUSTER(cluster_name)
```

3. Define a cluster sender channel to a full repo QM.

```
DEFINE CHANNEL(channel_name) CHLTYPE(CLUSSDR) CONNAME('remote_host_name_or_ip_address(port)') CLUSTER(cluster_name)
```

4. Define a cluster queue

```
DEFINE QLOCAL(qname) CLUSTER(cluster_name)
```

5. Test the cluster defined.


## How to refresh and reset a cluster

A cluster can be refershed with the below command

```
REFRESH CLUSTER
```

RESET cluster is used from full repository queue manager to forcibly remove a queue manager from its cluster.

```
RESET CLUSTER(clustername) QMNAME(qmname) ACTION(FORCEREMOVE) QUEUES(NO) 
```
