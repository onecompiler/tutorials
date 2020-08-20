* Having multiple methods with the same name is called method overloading.
* Method overloading is used to increase the readability of the program
* Consider we are performing the same operation for multiple length arguments, instead of using different names to each method we can use method overloading

## Types of method overloading
We can perform method overloading in 2 ways. They are:
1. Changing no of arguments
2. Changing the data type of arguments

## 1. Changing no of arguments
We change the number of arguments for each method so that object can differentiate based on the no of parameters
```java
import java.util.Date;

class Overload {
	//2 argumented add
  void add (int a, int b){
    System.out.println(a+b);
  }
  //3 argumented add
  void add (int a, int b, int c){
    System.out.println(a+b+c);
  }     
}

public class Example {
    public static void main(String[] args) {
        Overload ob = new Overload();
        ob.add(2,3);
        ob.add(1,2,3);
    }
}
```
### check output [here](https://onecompiler.com/java/3w49jwthy)

## 2. Changing data type of arguments
We can use the same method with different argument data types 
```java
class Overload {
  void add (int a, int b){
    System.out.println(a+b);
  }
  void add (double a, double b){
    System.out.println(a+b);
  }     
}

public class Example {
    public static void main(String[] args) {
        Overload ob = new Overload();
        ob.add(2,3);
        ob.add(1.4,2.3);
    }
}
```
### check output [here](https://onecompiler.com/java/3w49k9yhb)


