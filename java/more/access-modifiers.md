The Access Modifiers in Java specifies the scope of a method or field or class

There are 4 types of access modifiers in Java. They are :
1. Private
2. Public
3. Default
4. Protected

## Private
* It is accessible only inside the class

### Example
```java
class A{  
  private int a = 40;  
  private void show () {
    System.out.println("private method called");
  }  
}  
  
public class Simple{  
 public static void main (String args[]) {  
    A obj=new A();  
    System.out.println(obj.a);
    obj.show();
  }  
}  
```
The output will give an error as
```
a has private access in A
show() has private access in A
```
### check output [here](https://onecompiler.com/java/3w3ymeh8p)

## Public
* The data in public access modifier is accessible everywhere
### Example
```java
class A{  
  public int a=40;  
  public void show(){
    System.out.println("private method called");
  }  
}  
  
public class Simple{  
 public static void main(String args[]){  
   A obj=new A();  
   System.out.println(obj.a);
   obj.show();
   }  
}  
```
### check output [here](https://onecompiler.com/java/3w3ympxh6)

## Default
* The data in default is accessible only within package
* If no access modifier is present, it is treated as default by default
### Example
```java
// A.java
package p1;
class A {
	void show () {
		System.out.println("p1");
	}
}
```
```java
// B.java
package p2;
import p1.*;
class B {
	public static void main(String[] args){
		A ob = new A();
		ob.show();
	}
}
```
It will give an error as the scope is limited the package

## Protected
* It is accessible to all packages which are inherited
### Example
```java
// A.java
package p1;
public class A {
	protected void show () {
		System.out.println("p1");
	}
}
```
```java
// B.java
package p2;
import p1.*;
class B extends A {
	public static void main(String[] args){
		B ob = new B();
		ob.show();
	}
}
```
We will get output as `p1`.


