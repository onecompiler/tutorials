Python is an OOP(object oriented programming) language. It supports all the concepts of OOPS like Classes, objects, inheritance, encapsulation, abstraction, and polymorphism. 

# What is OOP

Object Oriented programming is a methodology based on objects instead of functions and procedures. OOP provides liberty to users to create objects as they want and create methods to handle the objects created. 

# OOP concepts

## 1. Classes and objects

Object is a basic unit in OOP, and is an instance of the class.

Class is the blueprint of an object, which is also referred as user-defined data type with variables and functions.

### How to create a Class

`class` keyword is required to create a class.

### Example

```py
class name:
    fname = "foo"
```
### How to create a Object

```py
n1 = name()
print(n1.fname)
```

## 2. Encapsulation

Encapsulation is a mechanism to protect private hidden from other users. It wraps the data and methods as a single bundle. `private` is the keyword used to declare the variables or methods as private. 

## 3. Abstraction

Data abstraction is a technique which provides only the required data to be visible or accessible to outside world. For example, making a call on a mobile. the logic behind making the call is not visible to others but it provides an abstract details of just call button to outside world.

## 4. Inheritance

Inheritance is a process of acquiring parent properties and behaviour by a child automatically. Hence you can either reuse or extend parent properties or behaviour in the child class. The main advantage of inheritance is code reusability.

### Syntax

```py
class parentclass:
    #code

class childclass(parentclassname):
    #code
```

## 5. Polymorphism

Polymorphism gives the meaning many forms, usually it occurs when multiple classes are present and have been inherited.

Consider an example of installing an application in your mobile, where your base class is `Mobile` and method is `installation()` and its derived classes could be installApp1, installApp2, installApp3 etc which will have its own implementation but installation method can be common which can be inherited from its base class.



