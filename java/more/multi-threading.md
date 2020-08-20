* A Thread is a lightweight sub-process
* Multithreading in Java is a way of executing multiple threads simultaneously
* Multithreading is used in games
* It helps in the maximum utilization of CPU

## Creation of Threads
Threads can be created in 2 ways. They are:

1. Extending to Thread class

2. Implementing a Runnable Interface

## 1. Extending Thread class
* We will extend our class to `java.lang.Thread`
* This class will override the `run()` method in Thread class
* To start the execution of a Thread we use `start()` method
* Threads starts their life inside `run()` method

### Example
```java
class Example extends Thread 
{ 
	public void run() 
	{ 
		try
		{ 
      System.out.println("current thread is " + Thread.currentThread().getId());
		} 
		catch (Exception e) 
		{ 
			System.out.println ("Exception"); 
		} 
	} 
} 

public class Main 
{ 
	public static void main(String[] args) 
	{ 
		int n = 5; 
		for (int i=0; i<n; i++) 
		{ 
			Example ob = new Example(); 
			ob.start(); 
		} 
	} 
} 
```
### check output [here](https://onecompiler.com/java/3w4cfmnux)

## 2. Implementing to a Runnable Interface
* We will create a class that implements the `java.lang.Runnable` interface

### Example

```java
class Example implements Runnable 
{ 
	public void run() 
	{ 
		try
		{ 
      System.out.println("current thread is " + Thread.currentThread().getId());
		} 
		catch (Exception e) 
		{ 
			System.out.println ("Exception"); 
		} 
	} 
} 

public class Main 
{ 
	public static void main(String[] args) 
	{ 
		int n = 5; 
		for (int i=0; i<n; i++) 
		{ 
			Thread ob = new Thread(new Example()); 
			ob.start(); 
		} 
	} 
} 
```
### check output [here](https://onecompiler.com/java/3w4cg2d43)