Function is a sub-routine which contains set of statements. Usually functions are written when multiple calls are required to same set of statements which increases re-usuability and modularity.

Functions allows you to divide your large lines of code into smaller ones. Usually the division happens logically such that each function performs a specific task and is up to developer.

Scala functions are the heart of Scala and that's why it is assumed to be a functional programming language.

## How to declare a Function

```java
def functionName ([argumentsList]) : [return type]
```

## How to define a Function
```java
def functionName ([argumentsList]) : [return type] = {
   // code
   return [expr]
}
```
`=` (equal to) operator is optional, If you use it then function will return some value else it will not return anything and will work similar to subroutine.

## How to call a Function

```java
function_name (argumentsList)
```

## Examples


## 1. Function with out `=` operator

```java
object function {
	def main(args: Array[String]): Unit = {
	greetings()
	}

  def greetings()  {        // Defining a function  with out = operator
    println("Good morning!!")  
  }
}
```
### Try Yourself [here](https://onecompiler.com/scala/3vwmsmwcv)

## 2. Function with out arguments and a return value.

```java
object Add {
	def main(args: Array[String]): Unit = {
	  println("sum of two numbers: " +sum());
	}
  
  def sum() : Int = { // defining function with = 
   var x, y, sum : Int = 0;
   x = 10;
   y = 20;
 
   sum = x + y;
   return sum; // returning sum value

}
}
```
### Try Yourself [here](https://onecompiler.com/scala/3vwmvdyhw)

## 3. Function with arguments and default values.

```java
object Add {
	def main(args: Array[String]): Unit = {
	  println("sum of two numbers with passing arguments: " + sum(10, 20));
	  
	  println("sum of two numbers with out passing arguments: " + sum());
	}
  
  def sum(x:Int = 1, y:Int = 1) : Int = { // defining function with default values
   var sum : Int =0;
   sum = x + y;
   return sum; // returning sum value

}
}

```

### Try Yourself [here](https://onecompiler.com/scala/3vwmw4zsc)

