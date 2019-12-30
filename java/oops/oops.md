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

## 2. Encapsulation

Encapsulation is a mechanism to protect private hidden from other users. It wraps the data and methods as a single bundle. `private` is the keyword used to declare the variables or methods as private. You can make public `set` and `get` methods to access private variables.

```java
public class Employee{
    public static void main(String args[]){
         EmployeeDetails o = new EmployeeDetails();
         o.setname("foo");
         o.setempId(12345);
         o.setsalary(20000);
         System.out.println("Employee Name: " + o.getname());
         System.out.println("Employee ID: " + o.getempId());
         System.out.println("Employee Salary: " + o.getsalary());
    } 
}

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
## Practice with more examples [here](https://onecompiler.com/java)


## 3. Abstraction

Data abstraction is a technique which provides only the required data to be visible or accessible to outside world. For example, making a call on a mobile. the logic behind making the call is not visible to others but it provides an abstract details of just call button to outside world. 

`abstract` keyword is used for classes and methods. 

* An abstract class cant be used for creating instances. In order to access, it must be inherited from other class. It can have both normal and abstract methods.
* An abstract method doesn't have body and it must be used inside an abstract class.

```java

abstract class Mobiles { // abstract class
  public abstract void features();   // abstract method
  public void inbuiltFeatures() {
    System.out.println("Voice and video calling");
    System.out.println("Dual Camera");
  }
}

class iPhone extends Mobiles {
  public void features() {
    System.out.println("Model : iPhone 11");
    System.out.println("Capacity : 256 GB");
    System.out.println("Liquid retina HD Display of 6.1 inches.");
    System.out.println("Dual 12MP Ultra Wide and Wide cameras");  }
}

class Mobile {
  public static void main(String[] args) {
    iPhone myMobile = new iPhone();
    myMobile.features();
    myMobile.inbuiltFeatures();
  }
}
```

## Practice with more examples [here](https://onecompiler.com/java)

## 4. Inheritance

Inheritance is a process of acquiring parent properties and behaviour by a child automatically. Hence you can either reuse or extend parent properties or behaviour in the child class. The main advantage of inheritance is code reusability. Use the keyword `extends` to inherit from a class.

### Type of Inheritance
* Single inheritance
    When a child inherits properties from a single parent

* Multiple inheritance
    When a child inherits its properties from two or more parents.

* Hierarchical inheritance
    When multiple children inherit properties from a parent

* Multilevel inheritance
    When a parent properties are inherited by a child which again get interited by another child. For example, B is inherited from A and C is inherited from B. 

    A -> B -> C
* Hybrid inheritance
    Hybrid inheritance is a combination or two or more inheritance types.


Below is an example of single inheritance

```java
class Apple {
  protected String brand = "Apple";        
  public void Features() {                    
    System.out.println("Capacity : 256 GB");
    System.out.println("Liquid retina HD Display of 6.1 inches.");
    System.out.println("Dual 12MP Ultra Wide and Wide cameras");
  }
}

class iPhone extends Apple {
  private String model = "iPhone 11";    
 
	public static void main(String[] args) {

    iPhone myMobile = new iPhone();

    System.out.println(myMobile.brand + "'s new realease is " + myMobile.model);
    myMobile.Features();

  }
}
```
## Practice with more examples [here](https://onecompiler.com/java)

## 5. Polymorphism

Polymorphism gives the meaning many forms, usually it occurs when multiple classes are present and have been inherited.

Consider an example of installing an application in your mobile, where your base class is `Mobile` and method is `installation()` and its derived classes could be installApp1, installApp2, installApp3 etc which will have its own implementation but installation method can be common which can be inherited from its base class.

```java
class News {
  public void alerts() {
    System.out.println("News Alerts");
  }
}

class Sports extends News {
  public void alerts() {
    System.out.println("subscribe Sports News");
  }
}

class Entertainment extends News {
  public void alerts() {
    System.out.println("Subscribe for Entertainment News");
  }
}

class Politics extends News {
  public void alerts() {
    System.out.println("Subscribe for politics alerts");
  }
}


class MainClass {
  public static void main(String[] args) {
    News myNews = new News(); 
    Sports mySports = new Sports();
    Entertainment myEntertainment = new Entertainment();
    Politics myPolitics = new Politics();
    myNews.alerts();
    mySports.alerts();
    myEntertainment.alerts();
    myPolitics.alerts();
  }
}
```

## Practice with more examples [here](https://onecompiler.com/java)
