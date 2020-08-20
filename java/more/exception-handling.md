* Exception handling is one of the powerful features of Java.  
* Exception is also known as Runtime error
* Examples for some runtime errors are ClassNotFoundException, IOException, etc.
* Exception handling is the process of handling those exceptions that occur to maintain the normality of the program.
* `java.lang.Throwable` is the root class of Java exception.
## Try and catch in Java
* The code that may have an exception is placed in `try` block
* The `catch` block catches the exception that is generated in the try block.
## Types of Exceptions in Java
There are majorly 2 types of exceptions in Java

1. Checked

2. Unchecked

## 1. Checked Exception
The classes which directly inherit `Throwable` class except the RuntimeException and error.
### Examples
IOException, SQLException, etc

## 2. Unchecked Exception
The classses which inherit RuntimeException class are unchecked exceptions.
### Examples
ArithmeticException, NullPointerException etc

## Examples
### ArithmeticException
```java
public class Example{  
  public static void main(String args[]){  
   try{  
      int b=5/0;//ArithmeticException  
   }
   catch(ArithmeticException e){
     System.out.println(e);
   }  
  }  
}  
```
### Check output [here](https://onecompiler.com/java/3w488bwdq)

### NullPointerException
### Example 1
```java
public class Example{  
  public static void main(String args[]){  
   try{  
      String name = null;  
      System.out.println(s.length());//NullPointerException
   }
  catch(NullPointerException e){
    System.out.println(e);
  }
  }  
}  
```
### Check output [here](https://onecompiler.com/java/3w488vpj8)

### Example 2
```java
public class Example{  
  public static void main(String args[]){  
   try{  
       int a[]=new int[5];
       System.out.println(a[9]);
   }
   catch(ArrayIndexOutOfBoundsException e){
     System.out.println(e);
   }
  }
}
```
### Check output [here](https://onecompiler.com/java/3w488yyyh)






