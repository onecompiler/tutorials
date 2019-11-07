## How to create a sample application manually

1. Click on `New Application` in Application Development view and provide name for it.
2. Click on New and then `Message flow` under your application created and provide name to it.
3. Now add nodes to the message flow by dragging the node from palatte.
4. consider you want to receive input from a queue and send the message to another queue after some transformation. 

```
MQInput --> Compute --> MQOutput
```

5. You can wire the connectivity between the nodes by simply creating a connection and connect source to target.
6. Also, specify the properties of each node in the properties view.
7. Your transformation logic can be written using ESQL in Compute node.
8. Save and create the bar file to deploy into a integration server.
9. You can deploy the bar file using any of the below methods
* Toolkit
* `mqsideploy` line command
* IIB Web user interface
* IIB Integration API.

## How to create a BAR file

You can create BAR file either using toolkit or command line.

1. Toolkit

* Click File >> New >> BAR file in the toolkit and specify its name
* Add the required components to the BAR file 
* Build and save it.

2. `mqsicreatebar` line command.


## How to deploy a BAR file 

1. After creating BAR file without any errors, you can just drag and drop the BAR file into the integration server or

2. Right click on the BAR file created, click deploy and specify the target integration server.

3. Check the deployment log for any errors.

4. Alternatively, you can also deploy the BAR file using web user interface. Go to the Integration server, right click, click on deploy and then select the BAR file. You can also change the BAR file properties while deploying.