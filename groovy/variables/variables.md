
Variables are like containers which holds the data values. A variable specifies the name of the memory location. 

## How to declare variables

There are three ways to declare variables in Groovy.

1. Native syntax similar to Java

```java
data-type variable-name = value;
```
2. Using def keyword

```java
def variable-name = value;
```
3. Variables in groovy do not require a type definition.

```java
variable-name = value; // no type definition is required
```

In the above syntax, assigning value is optional as you can just declare the variable and then assign value at later point in the program. Also, the value of a variable can be changed at any time.

## Naming convention of variables

* Variable's name should consists of only letters (both uppercase and lowercase letters), digits and underscore(`_`).
* Variable's name cannot contain white spaces like first name which is a invalid variable name.
* First letter of a variable should be either a letter or an underscore(`_`).
* Variable names are case sensitive and hence Name and name both are different.
* It is always advisable to give some meaningful names to variables.

### Example

```java
int x = 10; // declaring int variable and assigning value 10 to it
char grade = 'A'; // declaring char variable and assigning value A to it
```
The above can also be written like below

```java
int x; // declaring int variable 
char grade; // declaring char variable 

x = 10; // assigning value 10 to x variable
grade = 'A'; //and assigning value A to grade variable
```

### Sample program:

```java
int int_var = 99999 // using native method
println int_var;

int_var = 1000;
println int_var;

def x = "hello" // using def keyword
println x
```
### Check result [here](https://onecompiler.com/groovy/3vmpw47vt)