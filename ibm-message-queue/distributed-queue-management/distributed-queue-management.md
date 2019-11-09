Distributed queuing means sending messages from one queue manager to another queue manager which can exist on same system or some other system.

![](images/dqm.jpg)

Consider you are sending a message from QM1 to QM2 on your local machine where QM1 is running on 1415 and QM2 on 1416 ports. 

1. Create QM1 and start QM1 using the below commands

```
crtmqm QM1
```

```
strmqm QM1
```

```
runmqsc QM1
```

2. Create a remote queue, transmission queue and sender channel in QM1

Remote queue:

```
DEFINE QREMOTE ('TEST.QR') +
       DESCR('remote Q to send message to QL') +
       PUT(ENABLED) +
       DEFPRTY(0) +
       DEFPSIST(YES) +
       SCOPE(QMGR) +
       RQMNAME(QM2) +
       RNAME('TEST.QL') +
       XMITQ(XMITQ) 
```

Transmission queue:

```
DEFINE QLOCAL('XMITQ') USAGE(XMITQ)
```


Sender channel:

```
DEFINE CHANNEL ('QM1.QM2') CHLTYPE(SDR) DESCR('QM1 to QM2') TRPTYPE(TCP) XMITQ(XMITQ) CONNAME('localhost(1416)') 
```


2. Start the queue managers using below command.

```
crtmqm QM2
```
```
strmqm QM2
```
```
runmqsc QM2
```

Local Queue:

```
DEFINE QLOCAL('TEST.QL')
```

Receiver channel:

```
DEFINE CHANNEL ('QM1.QM2') CHLTYPE(RCVR) DESCR('QM1 to QM2')
```



Now if you send a message to the remote queue `TEST.QR`, you should receive it on `TEST.QL`.

## How to Test?

* Place a message by using the below command in command prompt.

```
./amqsput TEST.QR QM1
```

* Browse the local Queue to check if the message is reached.

```
./amqsbcg TEST.QL QM2
```