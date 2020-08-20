* If a subclass and parent class have the same method then it is known as Method overriding
* Used to provide the specific implementation of a method which already exists in its superclass

## Rules for method overriding
* The two classes must be in inheritance
* Final methods cannot be overridden
* Static methods cannot be overridden
* Private methods cannot be overridden
* The overriding method must have the same return type

### Example without overriding
```java

class A {  
  void show() {
    System.out.println("This is class A");
  }  
}  
 
class B extends A {  
  void show(){
    System.out.println("This is class B");
  }  
  
  public static void main(String args[]) {
	A ob1 = new A();  
    B ob2 = new B();
    ob1.show();
    ob2.show();
  }  
}  
```
### check output [here](https://onecompiler.com/java/3w49tfzk2)

### Example with overriding
```java

class A {  
  void show() {
    System.out.println("This is class A");
  }  
}  
 
class B extends A {  
  void show(){
    System.out.println("This is class B");
  }  
  
  public static void main(String args[]) {  
    B ob = new B();
    ob.show();
  }  
}  
```
### check output [here](https://onecompiler.com/java/3w49stjdp)
This will output `This is class B`
The `show()` method in class B overrides `show()` method in class A.
