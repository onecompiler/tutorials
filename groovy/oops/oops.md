Object Oriented programming is a methodology based on objects instead of functions and procedures. OOP provides liberty to users to create objects as they want and create methods to handle the objects created. Like in Java, Groovy also supports OOPs principles.

# 1. Classes and objects

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
* Nested class and interfaces

## How to create a Class

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
## How to create a Object

```java
mobile m1;
```
## How to define methods in a class

```java
public class greeting {
    static void hello(){
        println("Hello.. Happy learning!!");
    }

    public static void main(String[] args) {
    hello();
    }
}
```
## Practice with more examples [here](https://onecompiler.com/groovy)

# 2. Inheritance

Inheritance is a process of acquiring parent properties and behaviour by a child automatically. Hence you can either reuse or extend parent properties or behaviour in the child class. The main advantage of inheritance is code reusability. Use the keyword `extends` to inherit from a class.

## Type of Inheritance

* ### Single inheritance

    When a child inherits properties from a single parent

* ### Multiple inheritance

    When a child inherits its properties from two or more parents.

* ### Hierarchical inheritance

    When multiple children inherit properties from a parent

* ### Multilevel inheritance

    When a parent properties are inherited by a child which again get interited by another child. For example, B is inherited from A and C is inherited from B. 

    A -> B -> C
* ### Hybrid inheritance

    Hybrid inheritance is a combination or two or more inheritance types.

# 3. Encapsulation

Encapsulation is a mechanism to protect private data hidden from other users. It wraps the data and methods as a single bundle. `private` is the keyword used to declare the variables or methods as private. You can use public `set` and `get` methods to access private variables.

```java
EmployeeDetails o = new EmployeeDetails();
o.setname("foo");
o.setempId(12345);
o.setsalary(20000);
println("Employee Name: " + o.getname());
println("Employee ID: " + o.getempId());
println("Employee Salary: " + o.getsalary());
 
class EmployeeDetails{
    private String name;
    private int empId;
    private int salary;

    //Getter and Setter methods
    public int getempId(){
        return empId;
    }

    public String getname(){
        return name;
    }

    public int getsalary(){
        return salary;
    }

    public void setempId(int newId){
        empId = newId;
    }

    public void setname(String newName){
        name = newName;
    }

    public void setsalary(int newSalary){
        salary = newSalary;
    }
}
```
## Check result [here](https://onecompiler.com/groovy/3vmyukbxj)

# 4. Polymorphism

Polymorphism gives the meaning many forms, usually it occurs when multiple classes are present and have been inherited.

Consider an example of installing an application in your mobile, where your base class is `Mobile` and method is `installation()` and its derived classes could be installApp1, installApp2, installApp3 etc which will have its own implementation but installation method can be common which can be inherited from its base class.

```java
class News {
  public void alerts() {
    println("News Alerts");
  }
}

class Sports extends News {
  public void alerts() {
    println("subscribe Sports News");
  }
}

class Entertainment extends News {
  public void alerts() {
    println("Subscribe for Entertainment News");
  }
}

class Politics extends News {
  public void alerts() {
    println("Subscribe for politics alerts");
  }
}


News myNews = new News(); 
Sports mySports = new Sports();
Entertainment myEntertainment = new Entertainment();
Politics myPolitics = new Politics();
myNews.alerts();
mySports.alerts();
myEntertainment.alerts();
myPolitics.alerts();

```

## Practice with more examples [here](https://onecompiler.com/groovy/3vmyutmfg)

