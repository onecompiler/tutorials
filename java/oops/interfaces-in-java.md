* An interface in Java is a blueprint of a class, It contains static constants and abstract methods.
* Interfaces are mainly used to achieve abstraction and multiple inheritances
* Interfaces don't have a method body
* An interface can contain constants, default methods and static methods

## Difference between class and Interface

* An interface doesn't have the constructor
* All methods in Interface are abstract
* Instantiation of an interface is not possible
* It is implemented by class not extended

## Syntax

`interface` keyword is used to create an interface

```java
 interface Example {
	final int roll = 100;
	int display();
}
```

## Classes implementing interfaces

Interfaces contain abstract methods, so the definition isn't allowed inside interfaces. Classes perform the behavior of interfaces. Classes use the keyword `implements` to implement an interface.

### Example

```java

interface A {
  void read(int p);
}


class Example implements A {
  int a;
  public void read (int x){
    a = x;
  }
}

class Test {
  public static void main (String[] args){
    Example ob = new Example();
    ob.read(10);
    System.out.println(ob.a);
  }
}
```

## Extending Interfaces

An interface can extend another interface in the same way that classes do inheritance

### Example

```java
interface A {
  void read(int p);
}

interface B extends A {
  void show();
}

class Example implements B {
  int a;
  public void read (int x){
    a = x;
  }
  public void show () {
    System.out.println(a);
  }
}

class Test {
  public static void main (String[] args){
    Example ob = new Example();
    ob.read(10);
    ob.show();
  }
}

```
check result  [here](https://onecompiler.com/java/3w3vfsdgf)
