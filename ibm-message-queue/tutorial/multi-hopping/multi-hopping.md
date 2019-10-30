## What is Multi-hopping?

Passing a message from source to destination with one or more intermediate systems is called multi-hopping.

IBM MQ imeplements multi-hopping using queue manager alias

Consider you want to send a message from QM1 to QM3 through QM2(QM1->QM2->QM3). 

![](images/multi-hopping-3.jpg)

1. create the queue managers QM1, QM2, QM3 using `CRTMQM` and start them using `STRMQM`.

2. create a remote queue, transmission queue and sender chnanel in QM1

```
runmqsc QM1
```
Remote queue:

```
DEFINE QREMOTE ('QM1.QR') +
       DESCR('Remote Q to send message to QL in QM3') +
       PUT(ENABLED) +
       DEFPRTY(0) +
       DEFPSIST(YES) +
       SCOPE(QMGR) +
       RQMNAME(QM2) +
       RNAME('QM3.QL') +
       XMITQ(QM2.XMITQ) 
```

Transmission queue:

```
DEFINE QLOCAL('QM2.XMITQ') USAGE(XMITQ)
```


Sender channel:

```
DEFINE CHANNEL ('QM1.QM2') CHLTYPE(SDR) DESCR('QM1 to QM2') TRPTYPE(TCP) XMITQ(QM2.XMITQ) CONNAME('localhost(1416)') 
```


3. Create a queue manager alias, receiver channel to receive from QM1, transmission queue and sender channel to send message to QM3.

```
runmqsc QM2
```

Local Queue:

```
DEFINE QLOCAL('QM3.XMITQ') USAGE(XMITQ)
```

Receiver channel:

```
DEFINE CHANNEL ('QM1.QM2') CHLTYPE(RCVR) DESCR('QM1 to QM2')
```
Queue Manager Alias:
```
DEFINE QREMOTE ('QM3') +
       DESCR('Queue Manager alias') +
       PUT(ENABLED) +
       DEFPRTY(0) +
       DEFPSIST(YES) +
       SCOPE(QMGR) +
       RQMNAME(QM3) +
       RNAME('') +
       XMITQ(QM3.XMITQ) 
```

Transmission queue:

```
DEFINE QLOCAL('QM3.XMITQ') USAGE(XMITQ)
```


Sender channel:

```
DEFINE CHANNEL ('QM2.QM3') CHLTYPE(SDR) DESCR('QM2 to QM3') TRPTYPE(TCP) XMITQ(QM3.XMITQ) CONNAME('localhost(1417)') 
```
4. Create a  receiver channel to receive from Q2 and a local queue.

```
runmqsc QM3
```

Local Queue:

```
DEFINE QLOCAL('QM3.QL') 
```

Receiver channel:

```
DEFINE CHANNEL ('QM2.QM3') CHLTYPE(RCVR) DESCR('QM2 to QM3')
```


## Advantages of Multi-hopping

* Once the connectivity is established between the queue managers, you no need to create any objects at intermediate systems.
* You can just create objects at source and destination queue managers and you can send message from source to destination.