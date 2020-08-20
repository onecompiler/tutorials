## Java Abstract class
* An abstract class is a class that cannot be instantiated
* Which means we cannot create objects for an abstract class
* We use keyword `abstract` to create an Abstract class
* If we try to create object for an Abstract class we will get an error 
* To access methods of an Abstract class we need to create a subclass
* An abstract class can have both abstract and non-abstract methods

### Syntax
```java
abstract class Example {
	//methods
}
```
If we try to create object as we do for a normal class, the output will be an error as below

`Example is abstract; cannot be instantiated`

## Java Abstract Method
* An abstract method is created without the definition
* Abstract methods can only be created inside an Abstract class
* If we create an Abstract method inside a non-abstract class it will throw a error
### Example
```java
abstract class Example {
	abstract void read ();
	public void show () {
		System.out.println('Hey');
	}
}
```
## Inheriting an Abstract class
To access the attributes and methods of an abstract class we need to inherit to a class.
```java
abstract class A {
   abstract void show();
}

class B extends A {
   public void show() {
      System.out.println("function called");
   }
}
class Main {
   public static void main(String[] args) {
      B d1 = new B();
      d1.show();
   }
}
```
### check output [here](https://onecompiler.com/java/3w3yhgrf4)