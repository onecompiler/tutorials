## For

For loop is used to iterate a set of statements based on a condition, in short it is used to iterate, filter and return an iterated collection. 

### Syntax

```java
for(var <- range){  
//code  
} 
```
### Example

```java
object Loops {
	def main(args: Array[String]): Unit = {
	   for( x <- 1 to 10 ){  
       println(x);  
	   }
	}
}
```

### Try Yourself [here](https://onecompiler.com/scala/3vwfas3ad)

### Using until keyword

```java
object Loops {
	def main(args: Array[String]): Unit = {
	   for( x <- 1 until 10 ){  
       println(x);  
	   }
	}
}
```
### Try Yourself [here](https://onecompiler.com/scala/3vwfawe7p)

### For loop filtering

```java
object Loops {
	def main(args: Array[String]): Unit = {
	   for( x <- 1 until 10 if x % 2 != 0){  
       println(x);  
	   }
	}
}
```
### Try yourself [here](https://onecompiler.com/scala/3vwfb224d)

### For loop for collections like lists, sequence etc

```java
object Loops {
	def main(args: Array[String]): Unit = {
	 var l = List(10,20,30,40,50)  
   for( i <- l){             
      println(i)  
   }  
	}
}
```
### Try yourself [here](https://onecompiler.com/scala/3vwfb7e4p)

## While

While is also used to iterate a set of statements based on a condition. Usually while is preferred when number of iterations are not known in advance.

### Syntax

```java
while(condition){  
//code 
}  
```
### Example

```java
object Loops {
	def main(args: Array[String]): Unit = {
  	 var i : Int = 1;
     while ( i <= 10) {
        println(i);
        i = i+1;
     }

	}
}
```
### Try Yourself [here](https://onecompiler.com/scala/3vwfbdwe2)

## Do-while

Do-while is also used to iterate a set of statements based on a condition. It is mostly used when you need to execute the statements atleast once.

### Syntax

```java
do{  
//code 
}while(condition); 
```
### Example

```java
object Loops {
	def main(args: Array[String]): Unit = {
	    var i : Int = 1;
      do {
        println(i);
        i = i+1;
      } while (i<=10);
    
	}
}
```

### Try Yourself [here](https://onecompiler.com/scala/3vwfbk7n8)