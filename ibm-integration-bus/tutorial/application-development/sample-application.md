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

