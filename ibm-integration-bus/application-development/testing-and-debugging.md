## Testing Message flow

1. Integration Node, Integration server and your message flow should be created and ready.
2. You can test your message flow by using Test client in toolkit itself
3. You can test message flows for which input nodes must be any of the below
    * IBM MQ
    * JMS
    * HTTP
    * SOAP
    * SCA
4. Open Test Client editor either by right clicking on message node and click on `Test Message Flow` or open message flow, right click on input node, click Test.

5.  For MQ message flow testing, Click on configurations tab, and then MQ settings and select the options.

6. You can edit MQ header settings and also you can send MQRFH2 header 

7. You can either create a test message or import from a file system.

## Debugging Message Flow

1. Change the application development perspective to `Debug perspective`.
2. Add breakpoints to the message flow by right clicking and then click on Add Breakpoint 
3. You can also add breakpoint to your ESQL Or Java code.
4. Test with a message to check the variables or how your code is behaving.
5. Please note that only one user can use debugger at once to an integration server. Also, debugging reduces performance and it is usually carried in non-production environment.
6. You can also use `trace` node to debug your message flows.


