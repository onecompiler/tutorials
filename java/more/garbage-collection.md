* Garbage collection is the process of destroying unused objects
* In Languages like C/C++ we use `free()` or `delete()` to delete unused objects
* But in C/C++ user manually need to delete those objects but most of them don't do this.
* But in Java, this process is automatically done by the garbage collector in JVM
* It makes java memory efficient

## Methods in garbage collection
## 1. gc()
Used to start the cleanup process

### Syntax
```java
public static void gc() {
}
```
## 2. finalize()
This method is called each time when an object is garbage collected

### Syntax
```java
protected void finalize() {
}
```

An object can be deleted in 3 ways. They are

1. By assigning NULL
2. By assigning a reference to another object
3. Using an anonymous object

## 1. By assigning NULL
```java
Example ob = new Example();
ob = null;
```
### Example
```java
public class Example {  
  
 public void finalize() {
   System.out.println("object is garbage collected");
 }  
 
 public static void main(String args[]){  
  Example ob = new Example();  
  ob = null;  
  System.gc();  
 }  
}  
```
### check output [here](https://onecompiler.com/java/3w49hwdh3)

## 2. By assigning a reference to another object
```java
Example ob1 = new Example();
Example ob2 = new Example();
ob1 = ob2;
```
### Example
```java
public class Example {  
  
 public void finalize() {
   System.out.println("object is garbage collected");
 }  
 
 public static void main(String args[]){  
  Example ob1 = new Example();
  Example ob2 = new Example(); 
  ob1 = ob2;  
  System.gc();  
 }  
}  
```
### check output [here](https://onecompiler.com/java/3w49j38u2)

## 3. Using an anonymous object
```java
new Example ();
```
### Example
```java
public class Example {  
  
 public void finalize() {
   System.out.println("object is garbage collected");
 }  
 
 public static void main(String args[]){  
  new Example();
  System.gc();  
 }  
}  
```
### check output [here](https://onecompiler.com/java/3w49j6e8m)