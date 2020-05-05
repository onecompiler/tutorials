## 1. While

While is used to iterate a set of statements based on a condition. Usually while is preferred when number of iterations is not known in advance.

### Syntax

```java
while(condition){  
//code 
}  
```
### Example

```java
int i = 1;
while ( i <= 10) {
  println i;
  i++;
}
```
#### Check result [here](https://onecompiler.com/groovy/3vmssgpur)

## 2. For

For loop is used to iterate a set of statements based on a condition. Usually for loop is preferred when number of iterations are known in advance.

### Syntax

```java
for(Initialization; Condition; Increment/decrement){  
//code  
} 
```
### Example

```java
for (int i = 1; i <= 10; i++) {
  println i ;
}
```

#### Check Result [here](https://onecompiler.com/groovy/3vmssnvg8)


## 3. For..in

For..in is used to loop through ranges or set of values.

### Syntax

```java
for (var in range) {
  //code
}

```
### Example

```java
for (int i in 1..10 )
   println(i);
```

#### Check result [here](https://onecompiler.com/groovy/3vmssc6ny)