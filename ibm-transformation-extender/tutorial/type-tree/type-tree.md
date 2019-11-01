
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

## How to create a type tree manually

### Basics 

### 1. Class
    * Item
    Item represents smallest data object which doesn't contain other objects.

    * Group
    Group represents complex data objects which can contain other groups and items.
    There are three types of groups
    1. Unordered
    Unordered groups have components which can be of any order.

    2. Sequence
    Sequence groups have components which are partially ordered.

    3. Choice
    Choice group is like picking the group only when it mets the specified criteria of the component present in the group. It is like mulitple choice question, only one sub-group will have the data passed to it which mets the criteria.

    * Category
    Category organizes item and group types. 

### 2. Partitioned

If the data can be divided into mutually exclusive sub types, then you can partition the data.

### 3. Initiator

An initiator is a syntax object which comes at the beginning of the data. It can be literal or variable.

### 4. Terminator

A terminator is a syntax object which comes at the ending of the data. It can be literal or variable.

### 5. Item restrictions

You can specify restrictions whether they are valis or not valid data objects for an item type.


Now, as you are familiar with the basics, lets create a small type tree structure which has id and email details of 100 users.

1. Create a new type tree by clicking new type tree form the Extender navigator pane.
2. create two items id and email and you can specify the min and max size in its properties.
3. Create a group called user and drag id and e-mail fields to it. 
4. You can also specify other properties like padding, initiator and terminator, data language etc in the properties of group.
5. Drag the User group into new input group and set the range to user group as (1:100).
6. Now analyze the typetree. 