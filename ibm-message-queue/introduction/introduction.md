
IBM Message Queue(MQ), formerly knows as Websphere Message Queues is a family of message oriented middleware products. IBM MQ connects diverse applications and businesses across various platforms. MQ family includes IBM MQ, IBM MQ Advanced, IBM MQ Appliance, IBM MQ for z/OS, and IBM MQ on IBM Cloud.


## History

* Originally called as MQ Series, launched in 1993.
* Renamed to Websphere MQ in 2002.
* Renamed to IBM MQ in 2014.

## Key Features

* Cross Platform
* High availability, supports Replicated Data Queue Manager, Clustering, Multi-instance Queue Manager, Queue Sharing groups for Z/OS.
* Guaranteed one-time delivery
* Available as Client and server versions
* Wide EAI industry support 
* Asynchronous messaging
* Message-driven architecture framework
* Secure
* Follows FIFO(First In First Out), but messages can be prioritized.

## Terminology

1. Message
    * Message is a meaning full information
    * Message contain 2 parts i.e    MQMD(header part) and Application Data
2. Queue
    * Queue is used to store the messages
3. Queue Manager
    * It's a logical container for message queues.
    * Responsible for communication with other Queue managers.

## Installation 

### On windows using command prompt

1. Use the below command to install IBM MQ

```
msiexec /i "MQ_INSTALLATION_MEDIA\MSI\IBM WebSphere MQ.msi" /l*v c:\wmqinslogs\install.log AGREETOLICENSE="YES" 
```
2. Set up env variables

```
C:\Program Files\IBM\MQ\bin\setmqenv" -s
```
3. check if MQ is installed correctly. The below command should return version and other installation details.

```
dspmqver
```

 



