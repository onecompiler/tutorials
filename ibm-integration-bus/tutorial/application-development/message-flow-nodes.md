Message flow nodes are used to transform the incoming message and to provide routing control information and also to connect databases.

There are many in-built message nodes exist in IIB.

## i. Transport nodes

### 1. IBM MQ

* MQInput
* MQOutput
* MQReply
* MQGet
* MQHeader

### 2. JMS

* JMSInput
* JMSOutput
* JMSReceive
* JMSReply
* JMSHeader
* JMSMQTransform
* MQJMSTransform

### 3. Web Services

* SOAPInput
* SOAPReply
* SOAPRequest
* SOAPAsyncRequest
* SOAPAsyncResponse
* SOAPEnvelope
* SOAPExtract
* RegistryLookup
* EndpointLookup

### 4. HTTP

* HTTPAsyncRequest
* HTTPAsyncResponse
* HTTPInput
* HTTPReply
* HTTPRequest
* HTTPHeader

### 5. IBM MQ Managed File Transfer

* FTEInput*
* FTEOutput*

### 6. File

* FileInput
* FileOutput
* FileRead
* CDInput*
* CDOutput*

### 7. SCA

* SCAInput
* SCAReply
* SCAAsyncRequest
* SCAAsyncResponse
* SCARequest

### 8. TCP/IP

* TCPIPClientInput
* TCPIPClientOutput
* TCPIPClientReceive
* TCPIPServerInput
* TCPIPServerOutput
* TCPIPServerReceive

### 9. Other external systems

* CORBARequest
* CICSRequest
* IMSRequest

### 10. WebSphere Adapters

* JDEdwardsInput
* JDEdwardsRequest
* PeopleSoftInput
* PeopleSoftRequest
* SAPInput
* SAPReply
* SAPRequest
* SiebelInput
* SiebelRequest

## ii. Routing

* Filter
* Route
* Label
* RouteToLabel
* Publication
* AggregateControl*
* AggregateReply*
* AggregateRequest*
* Collector*
* Resequence*
* Sequence*

## iii. Transformation

* Compute node
* JavaCompute
* XSLTransform
* Mapping
* .NETCompute

## iv. Database

* Database
* DatabaseInput
* DatabaseRetrieve
* DatabaseRoute

## v. Construction

* Input
* Output
* Throw
* Trace
* TryCatch
* FlowOrder
* Passthrough
* ResetContentDescriptor         

## vi Timer

* TimeoutControl*
* TimeoutNotification*

## vii Email

* EmailInput
* EmailOutput

