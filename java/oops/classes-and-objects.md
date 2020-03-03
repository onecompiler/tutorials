Object Oriented programming is a methodology based on objects instead of functions and procedures. OOP provides liberty to users to create objects as they want and create methods to handle the objects created. 

## OOP Advantages

* faster
* easier to understand
* Reusability
* easier to maintain, modify and debug

## OOP concepts

## 1. Classes and objects

Object is a basic unit in OOP, and is an instance of the class.

Class is the blueprint of an object, which is also referred as user-defined data type with variables and functions.

### How to create a Class

`class` keyword is required to create a class.

### Example


```java
class mobile {
    public:    // access specifier which specifies that accessibility of class members 
    string name; // string variable (attribute)
    int cost; // int variable (attribute)
};

```
### How to create a Object

```java
mobile m1;
```
### How to define methods in a class

```java
public class greeting {
    static void hello(){
        System.out.println("Hello.. Happy learning!!");
    }

    public static void main(String[] args) {
    hello();
    }
}

```
## Practice with more examples [here](https://onecompiler.com/java)
