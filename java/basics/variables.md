Variables are like containers which holds the data values. A variable specifies the name of the memory location. 

## How to declare variables

```java
data-type variable-name = value;
```
In the above syntax, assigning value is optional as you can just declare the variable and then assign value at later point in the program.

## Naming convention of variables in Java

* In Java, Variable names are case sensitive and hence Name and name both are different.
* variable name should begin with a `lower case letter` as per the java coding standards and if the variable name contains more than one word then second word should be a capital like this: firstName, pinCode etc.
* Variables naming cannot contain white spaces like first name which is a invalid variable name.
* special characters like `$` and `_` can be used to begin a variable name.

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

## Types of Variables

### 1. Local Variables

Local variables are declared inside the body of a method. Life of the local variable will be only with in the method it is declared.

### 2. Instance Variables

Instance variables are declared inside the class but outside methods. These are called instance variables because the values of the instance variables are instance specific and are not shared with other instances.

### 3. Static Variables

Static variables are declared with static keyword. They are also known as Class variables as they are associated with the class and are common for all the instances of the class.

### Example

```java
Class Sum {
    int n1 = 10; // Instance  Variable
    static int n2 = 20; //static variable
    void sum(){
        int n3 = 30; //local variable
        int total = n1+n2+n3;
    }
}
```
## Practice with more examples [here](https://onecompiler.com/java)