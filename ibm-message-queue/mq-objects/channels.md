
Channel is like a logical communication link between two parties. There are two types of channels present in IBM MQ.

## 1. Message Channels

* connects two Queue managers(QM) using MCA(Message Channel Agent)
* Uni-Directional
* Both channels defined at two Queue managers(QM) should have identical names.
* Widely used protocol is TCP/IP

## Possible subtypes of Message channels

### 1. Sender -Receiver channel

* Used to send messages from one QM to other QM.
* Sender and Receiver channel names must be same.

### Syntax

Sender channel:

```
DEFINE CHANNEL ('channel.name') CHLTYPE(SDR) DESCR('description') TRPTYPE(TCP) XMITQ(xmitq.name) CONNAME('hostname(portno)') 
```

Receiver channel:

```
DEFINE CHANNEL ('channel.name') CHLTYPE(RCVR) DESCR('description')
```

### 2. Server - Requester channel

* Used to send messages from one QM to other QM.
* Sender and Requester channel names must be same.
* You can also use Server-Receiver channels
* Requester can initiate the communication

### Syntax

Server:
```
DEFINE CHANNEL ('channel.name') CHLTYPE(SVR) DESCR('description') TRPTYPE(TCP)
XMITQ(xmitq.name) CONNAME('hostname(portno)')
```

Requester:
```
DEFINE CHANNEL ('channel.name1') CHLTYPE(RQSTR) DESCR('description') CONNAME('hostname(portno)')
```

### 3. Cluster sender - Cluster Receiver channel

* Used to send messages from a cluster queue manager to other queue managers in the cluster.
* For example, a full repository queue manager sends cluster status like addition/deleting of MQ objects to other Queue managers using this type of channels.

### Syntax

Cluster Sender channel:
```
DEFINE CHANNEL ('channel.name') CHLTYPE(CLUSSDR) TRPTYPE(TCP) CONNAME('hostname(portno)') CLUSTER('cluster.name')
```

Cluster Receiver channel:
```
DEFINE CHANNEL ('channel.name') CHLTYPE(CLUSRCVR) DESCR('description') CLUSTER('cluster.name') CONNAME('hostname(portno)')
```
## 2. MQI channels

* These channels are used to connect a IBM MQ Client to a Queue manager or IBM MQ Server.
* Bi-Directional

### 1. Server connection channel

* Used to connect a IBM MQ client to a IBM MQ server
* It is a bi-directional channel defined at server end.

### Syntax
```
DEFINE CHANNEL ('channel.name') CHLTYPE(SVRCONN) DESCR('description')
```

### 2. Client connection channel
* Used to connect a IBM MQ client to a IBM MQ server
* It is a bi-directional channel defined at client end.

### Syntax
```
DEFINE CHANNEL ('channel.name') CHLTYPE(CLNTCONN) CONNAME('hostname(portno)') TRPTYPE(TCP) DESCR('description')
```