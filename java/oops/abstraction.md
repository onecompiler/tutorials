
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

