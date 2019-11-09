Queue Manager provides queuing services to applications. 

   * Handles Storage, triggering, security, timing issues etc.
    * Default port is 1414
    * can send one message to many destinations.
    * Allows user either use command line or graphical(explorer) controlling to create, delete, manage queues and MQ objects.
    * Need atleast one listener to be up and running.
    * IBM MQ objects are case sensitive and can be up to 48 characters long.


## How to create a Queue manager(QM)?
The below command is used to create a queue manager

### Syntax
```
CRTMQM QM_NAME -u DLQ.NAME -p portnumber -d default.XMITQ
```

## How to start a Queue manager(QM)?
The below command is used to start the Queue manager

```
STRMQM QM_NAME
```

## List the Queue managers present
The below command is used to display the available queue manager names and it's status present on the system.

```
dspmq
```

## How to delete a Queue manager(QM)?
The below command is used to delete the queue manager created.

```
DLTMQM QM_NAME
```

## Note:
You must run the below command to  create or manage MQ objects present inside the queue manager.

```
runmqsc QM_NAME
```