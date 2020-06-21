Scala provides rich set of collection libraries which contains classes and traits to collect data. These collections can be mutable or immutable in nature. 

All the mutable collections are present in `Scala.collection.mutable` package and all the immutable collections are present in ` Scala.collection.immutable` package.

Some of the most commonly used collections are as follows:

## 1. Lists

Lists are used to store ordered elements. May contain duplicate elements and generally good for last-in-first-out (LIFO), stack-like access patterns.

Lists are very much similar to arrays.

### Example

```java

object Lists {
	def main(args: Array[String]): Unit = {
	  var listOfNumbers: List[Int]  = List(10, 40, 20, 80, 60, 200, 150);
    println(listOfNumbers);
	}
}
```
### Try yourself [here](https://onecompiler.com/scala/3vwnhtt2t)

## 2. Sets

Sets are used to store unique elements and they don't maintain any order. They can be mutable or immutable. 

### Example

```java

object Sets {
	def main(args: Array[String]): Unit = {
	  var mobiles = Set("iPhone", "Samsung", "OnePlus", "Nokia");
    println(mobiles);
	}
}
```
### Try yourself [here](https://onecompiler.com/scala/3vwnj78n9)

## 3. Maps

Maps are used to store key-value pairs. You can either use `,` or `->` operators to specify key-value pairs as shown below. Notice you need to enclose key-value pair in `()` when you use `,`.

### Example

```java

object Maps {
	def main(args: Array[String]): Unit = {
	  var mobiles1 = Map(("iPhone", " iPhone 11 pro"), ("Samsung", "Galaxy S20"));
	  
	  var mobiles2 = Map("iPhone" -> " iPhone 11 pro", "Samsung"-> "Galaxy S20");
    println(mobiles1);
    println(mobiles2);
	}
}
```
### Try yourself [here](https://onecompiler.com/scala/3vwnjd4m8)

## 4. Tuples

Tuples  are used to store ordered elements of different types like you can mix interger values with string values and float values etc. These values can be of same type as well.

### Example

```java

object Tuples {
	def main(args: Array[String]): Unit = {
	  var tup = ("iPhone", 1, "$1299", 5.6);
	  
    println(tup);
	}
}
```
### Try yourself [here](https://onecompiler.com/scala/3vwnju37g)

