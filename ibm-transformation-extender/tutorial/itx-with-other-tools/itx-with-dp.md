IBM transformation extender(ITX) maps can be used in IBM data power for complex data transformation like when you are dealing with EDI(Electronic Data Interchange). 

## How to create map for data power

1. Create the map in ITX 
2. In the map settings, change the Map Runtime to DataPower instead of Transformation Extender.
3. compile the map to get `.dpa` file.
4. Now, You can use the dpa file in Data power using `Pipeline policy`.
5. You can use the ITX map in either request or response. 
6. You can modify the component to upload the .dpa file.