 
IBM Transformation eXtender is a powerful data transformation middleware tool offered by IBM.

It is widely known for transformation of any format(CSV,EDI,EDIFACT,XML etc) to any desired format so that it acts as a middleware between source and target.

ITX can also be integrated with several other middleware tools like IBM Integration Bus(IIB), IBM Data Power, Sterling Integrator.

## Terminology

1. Design Studio

Design studio provides eclipse based GUI where you can create, test and performance tune your maps.

2. Type tree

Type tree is a graphical data dictionary which contain metadata definitions of input and output structures. They can either manually created or automatically by importing known metadata like XSDs, COBOL copybooks, IDocs, database catalogs etc. With this you can visualize the data structure of input and output. Type Designer is the component where you create and manage type trees.

3. Maps

Map is the source code where you write business logic in the form of rules. Map designer helps you to create maps and debug, test and build executable files for different platforms like Windows, IBM Z/OS, Linux, Sun solaris etc.

4. Integration Flow Designer(IFD)

IFD is a component of Design studio, where you can combine one or more maps and make them work as a single unit. You can also specify the properties of the map and these will override the map settings you make at map designer level. 

For example, you might have specified local file path as input path at map designer level and MQ at IFD level. IFD settings will take precedence and map will run when ever a new message comes to the configured queue.

5. Database Interface Designer(DID)

DID is a component of Design studio, which helps in importing metadata about queries, tables, and stored procedures in relational databases.

