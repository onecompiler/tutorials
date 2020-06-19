# If Family

When ever you want to perform a set of operations based on a condition(s) IF/IF-ELSE/Nested ifs are used.

You can also use if-else for nested IFs and IF-ELSE-IF ladder when multiple conditions are to be performed on a single variable.

## 1. If

### Syntax

```java
if(conditional-expression)
{
    //code
}
```
### Example

```java
object conditionalStatements {
 def main(args: Array[String]): Unit = {
	 var x : Int= 30;
   var y : Int= 30;

   if ( x == y) {
      println("x and y are equal");
   }
 }
}
```
### Check result [here](https://onecompiler.com/scala/3vwfa4sy4)

## 2. If-else

### Syntax

```java
if(conditional-expression)
{
    //code
} else {
    //code
}
```
### Example

```java
object conditionalStatements {
 def main(args: Array[String]): Unit = {
	 var x : Int= 30;
   var y : Int= 20;

   if ( x == y) {
      println("x and y are equal");
   } else {
      println("x and y are not equal");
   }
 }
}
```
### Check result [here](https://onecompiler.com/scala/3vwfa8p6z)

## 3. If-else-if ladder

### Syntax
```java
if(conditional-expression-1)
{
    //code
} else if(conditional-expression-2) {
    //code
} else if(conditional-expression-3) {
    //code
}
....
else {
    //code
}
```

### Example
```java
object ConditionalStatements {
	def main(args: Array[String]): Unit = {
 	  var age : Int = 15;

	  if ( age <= 1 && age >= 0) {
	    println("Infant");
	  } else if (age > 1 && age <= 3) {
	    println("Toddler");
	  } else if (age > 3 && age <= 9) {
	    println("Child");
	  } else if (age > 9 && age <= 18) {
	    println("Teen");
	  } else if (age > 18) {
	    println("Adult");
	  } else {
	    println("Invalid Age");
	  }
	}
}
```
### Check result [here](https://onecompiler.com/scala/3vwfadghg)

