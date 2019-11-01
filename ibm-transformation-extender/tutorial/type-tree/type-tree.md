
Type tree is the graphical representation of structure of data of input and output. Type designer is the component which helps in 

* Create and manage type trees

* Add data validation like age cant be a negative number

* You can automatically create type tree by importing metadata such as XML Schema Definition(XSD), Document Type Definitions (DTDs), WSDL, COBOL Copybooks, and Enterprise Java Beans (EJBs) etc.

* You can also compare two type trees which help you in analyzing the type trees.


## How to create a type tree by importing XSD.

1.  Create a new Extender project 
2. Expand the project in the Extender navigator
and right click on type trees
3. Select import -> XML Schema (here you can also select any other metadata definition like WSDL, IDoc, EJB etc)
4. Select the XSD
5. Choose either Xerces or classic parser validation.
Xerces: real xml parser where the data will get validated against XSD during run-time.
classic: classic parser transforms XSDs to plain type tree.
6. Provide type tree name and finish.



