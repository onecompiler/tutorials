* Binding is the process of connecting the function call to the function body

There are 2 types of binding. They are:
1. Static
2. Dynamic

## 1. Static Binding
* Type of the object is found at compile time
* If there is a private, static or final method in a class, then static binding takes place

### Example
```java
class Example {  
 private void show() {
   System.out.println("Hello world");
 }  
  
 public static void main(String args[]) {  
  Example ob = new Example();  
  ob.show(); 
 }  
}  
```
### check output [here](https://onecompiler.com/java/3w4cgsx3v)

## 2. Dynamic Binding
* Type of the object is found at run time

### Example
```java
class A {  
 void show() {
   System.out.println("This is class A");
 }  
}  
  
class B extends A {  
 void show() {
   System.out.println("This is class B");
 }  
  
 public static void main(String args[]){  
  A ob = new B();  
  ob.show();  
 }  
}  
```
### check output [here](https://onecompiler.com/java/3w4cgxe3a)
