##  Types of Queues


## 1. Local Queue
* Messages are stored physically
* All other Queues must be defined to point to a local queue.

### Syntax
```
DEFINE QLOCAL('QUEUE.NAME') DESCR('Description') 

```

Local Queue can act as below special queues

### 1. Transmission Queue(XMITQ)
  * Local queue with USAGE(XMITQ) attribute
  * Directs remote queue messages to other remote Queue Manager.
  * If the channel is down/broken, then messages will be present in XMITQ.

### Syntax
```
DEFINE QLOCAL ('XMITQ.NAME')  DESCR('Description') +
TRIGDATA ('CHANNEL.NAME') +
INITQ ('SYSTEM.CHANNEL.INITQ') +
USAGE(XMITQ) +
```

### 2. Dead Letter Queue(DEADQ) 
  *  Used for the unpredictable situations when Queue manager is unable to deliver messages to the destination point.
#### Situations:
   * When target queue is unavailable or full
   * when target queue doesn't exist
   * Authorization issues
   * Message is very big

### Syntax
```
DEFINE QLOCAL('DLQ.QUEUE.NAME') DESCR('Description') 

```

### 3. Intiation Queue(INITQ)

* Queue manager writes a trigger message using this queue to initiate an action like initiates the channel to start sending messages to another queue manager.

### Syntax
```
DEFINE QLOCAL('INITQ.NAME') DESCR('Description') 

```

### 4. Cluster Queue 
* Used for advertizing its existence with other queue managers present in the cluster.

### Syntax
```
DEFINE QLOCAL('QUEUE.NAME') DESCR('Description') CLUSTER('CLUSTER.NAME') SHARE 

```


## 2. Remote Queue
* Points to a local queue of remote Queue manager.
 

### Syntax

```
DEFINE QREMOTE ('QREMOTE.NAME') +
       DESCR('Description') +
       PUT(ENABLED) +
       DEFPRTY(0) +
       DEFPSIST(YES) +
       SCOPE(QMGR) +
       RQMNAME(RQMN.NAME) +
       RNAME('RQM.LOCALQ.NAME') +
       XMITQ(XMITQ.NAME) +
       DEFBIND(OPEN) 
```

## 3. Model Queue

* It's a template to create a real queue dynamically.

```
DEFINE QMODEL('MODELQ.NAME') +
DESCR('Description') +
MAXMSGL(4194304) +
MAXDEPTH(500000) +
PROCESS(' ') 
```

## 4. Alias Queue

* Pointer to a local queue present in the same Queue manager.
* It can be a pointer to a topic for pub/sub applications.
* Convinient way to hide real queue names from other applications to connect with.

```
DEFINE QALIAS('ALIASQ.NAME') +
DESCR('Description') +
TARGET('LOCALQ.NAME') 

```

#### Note
All the above syntax are for basic understanding only. Attributes for each command can be added based on the requirement.