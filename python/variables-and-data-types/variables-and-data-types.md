
# Variables

Variables are used to store data values.

In Python, declaring variables is not required. Means you don't need to specify whether it is an integer or string etc as Python is a dynamically typed language.

### Rules for naming variables:
* They must start with a letter or underscore. 
* They can not start with number
* Case-sensitive
* To create a global variable inside a function, global keyword is used.


# Data Types

|Category|Data Type|
|----|-----|
|Text |str|
|Number|int, float, complex|
|Boolean|bool|
|Binary|bytes, bytearray, memoryview|
|Set|set, frozenset|
|Sequence|list, tuple, range|
|Mapping|dict|

* type() is used to know the data type of a variable

## Data casting

|Constructor function| desc|
|-----|----|
|int()| constructs an integer from any form of data like string, float or integer|
|float()|constructs a float number from any form of data like string, float or integer|
|str()|constructs a string from any form of data like string, float or integer|

## Example

```python
name    = "Foo"       # String assignment
number = 10          # An integer assignment
cost   = 10.70       # A floating point assignment

print (type(name)) # to Print the data type of the variable name
print (type(number))  # to Print the data type of the variable number
print (type(cost))  # to Print the data type of the variable cost

print ("Hi " + name + "! You have purchased " + str(number) + " fruits and your total bill is " + str(cost) + "dollars") 
# here number and cost are integers and float values and you need to convert them to string in-order to print along with other string values.
```