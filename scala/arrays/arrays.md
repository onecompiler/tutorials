Array is a collection of similar data which is stored in continuous memory addresses. They are used to store multiple values in a single variable. Array values can be fetched using index. 

## Syntax

### Declaring 1-D array

```java
var arrayName:Array[datatype] = new Array[datatype](size)

//or

var arrayName = new Array[datatype](size)
```

### Declaring 2-D array

```java
var arrName = ofDim[datatype](rowsize,colSize)
```
## Example

```java
 var arr1 = Array(1, 2, 3, 4, 5)
```

## How to access Array elements

Array elements can be accessed using index. Index starts from 0 to size-1.

```java
object Arrays {
	def main(args: Array[String]): Unit = {
	  var mobiles = Array("iPhone", "Samsung", "OnePlus", "Nokia");
    println(mobiles(1));
	}
}
```
### Check result [here](https://onecompiler.com/scala/3vwn2w22d)

Please note that you have to use () instead of [] while accessing an array element. Here, we are using `mobiles(1)` but not `mobiles[1]` which we usually do in other languages.

## How to change a particular element in an array

To change a specific element in an array, refer it's index. 


```java

object Arrays {
	def main(args: Array[String]): Unit = {
	  var mobiles = Array("iPhone", "Samsung", "OnePlus", "Nokia");
    mobiles(2) = "Oppo";

    // Prints all the array elements
    for ( i <- mobiles ) {
       println( i )
    }
  }
}
```
### Check Result [here](https://onecompiler.com/scala/3vwn356vg)

