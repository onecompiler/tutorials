IBM Integration Bus(IIB) is an Enterprise Service Bus(ESB) which provides connectivity across enterprise systems and applications.

IIB is a powerful middleware tool by IBM where it transforms and routes the data from any where to any where.

* Supports different types of protocols and data formats.
* You can route, filter, transform, enrich, monitor, distribute, correlate, and detect the data
* You can perform mapping by using graphical mapping, Java, ESQL, and XSL
* Supports publish-subscribe along with IBM MQ.
* Can create and use reusuable patterns for generic solutions.


## Components of IIB

### Integration Node

* Routes, transforms, and enriches the in-flight messages as determined by the message
flows and their message models
* Also, known as runtime engine of IIB.
* there can be many integration nodes, which can run on different systems to avoid system failures.

### Integration server

* Group of message flows which are assigned to a integration node.

### Message flow

* Defines sequence of operations which are to be performed on the data.
* Message flows are used to route and transform the message.

### IIB Integration Toolkit

* An eclipse based GUI for integrated development environment. 
* Deploy the message flows on to Integration Nodes.
* Supported both in windows and linux.

### IBM Integration API

* Java administration API
* Used to control integration nodes and their resources remotely like deploying flows, change properties, check logs etc.

### Integration Project

* Container where you create and maintain all the resources like Message flows, Subflows, Message maps,ESQL files, Database definitions, BAR files.
