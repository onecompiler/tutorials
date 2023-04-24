Inheritance is a process of acquiring parent properties and behaviour by a child automatically. Hence you can either reuse or extend parent properties or behaviour in the child class. The main advantage of inheritance is code reusability. Use the keyword `extends` to inherit from a class.

### Type of Inheritance

## 1. Single Inheritance

When a child inherits properties from a single parent

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

## 2. Hierarchical inheritance

When multiple children inherit properties from a parent

Below is an example of hierarchical inheritance

```java

class A {
	public void print_A() 
  { 
    System.out.println("Class A"); 
  }
}

class B extends A {
	public void print_B() 
  { 
    System.out.println("Class B");
  }
}

class C extends A {
	public void print_C() 
  {
    System.out.println("Class C");
  }
}

class D extends A {
	public void print_D() 
  {
    System.out.println("Class D");
  }
}

public class Test {
	public static void main(String[] args)
	{
		B obj_B = new B();
		obj_B.print_A();
		obj_B.print_B();

		C obj_C = new C();
		obj_C.print_A();
		obj_C.print_C();

		D obj_D = new D();
		obj_D.print_A();
		obj_D.print_D();
	}
}

```

## 3. Multilevel inheritance

When a parent properties are inherited by a child which again get interited by another child. For example, B is inherited from A and C is inherited from B. 

A -> B -> C

```java

class A {
	public void printA()
	{
		System.out.println("Class A");
	}
}

class two extends one {
	public void printB() 
  { 
    System.out.println("Class B"); 
  }
}

class three extends two {
	public void printC()
	{
		System.out.println("Class C");
	}
}

// Drived class
public class Main {
	public static void main(String[] args)
	{
		three g = new three();
		g.printA();
		g.printB();
		g.printC();
	}
}

```

## 4. Multiple inheritance

When a child inherits its properties from two or more parents. It can be done by the help of Interface.

Below is an example of multiple inheritance.

```java

interface one {
	public void printA();
}

interface two {
	public void printB();
}

interface three extends one, two {
	public void printC();
}
class child implements three {
	public void printA()
	{
		System.out.println("Interface");
	}

	public void printB() 
  { 
    System.out.println("Class B"); 
  }

  public void printC() 
  { 
    System.out.println("Class C"); 
  }
}

public class Main {
	public static void main(String[] args)
	{
		child c = new child();
		c.printA();
		c.printB();
		c.printC();
	}
}

```


## 5. Hybrid inheritance

  Hybrid inheritance is a combination or two or more inheritance types.

  Below is the example of hybrid inheritance.

  ```java

public class ClassA 
{

}
public class ClassB extends ClassA
{

}
public class ClassC extends ClassA 
{

}
public class ClassD extends ClassB 
{
  
}

  ```



## Practice with more examples [here](https://onecompiler.com/java)



