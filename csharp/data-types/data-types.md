As the name suggests, data-type specifies the type of the data present in the variable. Variables must be declared with a data-type. 

There are three categories od data types in C#.


## 1.Value Data types

There are two types of value Data types:

### i. Pre-defined Data types
| Data type | Description | Range | Memory Size|
|----|----|----|----|----|
|byte|	used to store unsigned integer|	0 to 255| 1 byte|
|sbyte|	used to store signed integer|	-128 to 127| 1 byte|
|short|	used to store signed integers|	-32,768 to 32,767| 2 bytes|
| int| used to store signed integers|-2,147,483,648 to 2,147,483,647|4 bytes| 
|long | used to store signed integers|-9,223,372,036,854,775,808 to 9,223,372,036,854,775,807| 8 bytes|
|float| used to store fractional numbers|6 to 7 decimal digits| 4 bytes|
|double| used to store fractional numbers|15 decimal digits| 8 bytes|
|char|used to store a single character enclosed in single quote|one character|2 bytes|
|bool| Boolean data type|Stores either true or false | 1 bit|


### ii. User-defined data types

User-defined data types are like Structure, Enumerations, etc which will be discussed in future chapters.

## 2. Reference Data types

The reference data types do not contain the actual data stored in a variable like the value data types instead  they hold a reference to the variables.

There are 2 types of reference data types:

### i. Predefined Types 

| Data type | Description | Range | Memory Size|
|----|----|----|----|----|
|String| Stores a sequence of characters enclosed in double quotes| Sequence of Characters|2 bytes per character|
|object | Object can be represented as base type of all other types.|||


### ii. User defined Types

User-defined data types are like Classes, Interfaces etc.

Let's discuss more in detail about Reference Data types in future chapters.

## 3. Pointer Data type

Pointer is a variable which holds the memory information(address) of another variable of same data type.

```c#
datatype *pointername;
// or
datatype* pointername;
```
### Examples

```c#
int* ptr1, ptr2;   // valid
int *ptr1, *ptr2;   // Invalid
int x = 10;
int *ptr = &x; // declaring pointer variable and assigning x address location
```

