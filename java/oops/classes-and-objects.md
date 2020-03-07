Object Oriented programming is a methodology based on objects instead of functions and procedures. OOP provides liberty to users to create objects as they want and create methods to handle the objects created. 

## OOP Advantages

* faster
* easier to understand
* Reusability
* easier to maintain, modify and debug


## 1. Classes and objects

Object is a basic unit in OOP, and is an instance of the class. They have state and behaviour. Consider mobile as an object and let's see an example to understand state and behaviour. 

State of the objects can be represented by variables and behaviour of the objects can be represented by methods. Object is a real-world or run-time entity.

**Object** : Mobile
**State** : Model, Brand, Color, Size
**Behaviour** : Make a call, send a message, Click a picture

Class is the blueprint of an object, which is also referred as user-defined data type with variables and functions. Class is a logical entity.

Class can contain:

* Fields or variables
* Methods
* Constructors
* Blocks
* Nested class and interfaces

### How to create a Class

`class` keyword is required to create a class.

```java
class class_name {  
    fields;  
    methods;  
}
```
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
