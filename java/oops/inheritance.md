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

