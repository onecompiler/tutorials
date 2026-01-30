## For

For loop is used to iterate a set of statements based on a condition. Usually for loop is preferred when number of iterations are known in advance.

### Syntax

```java
for(Initialization; Condition; Increment/decrement){  
//code  
} 
```
### Example

```java
public class ForExample {
    public static void main(String[] args) {
      for (int i = 1; i <= 5; i++) {
        System.out.println(i);
      }
    }
}
```

#### Check Result [here](https://onecompiler.com/java/3vk6bxq29)

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
public class WhileExample {
    public static void main(String[] args) {
      int i=1;
      while ( i <= 5) {
        System.out.println(i);
        i++;
      }
    }
}
```
#### Check result [here](https://onecompiler.com/java/3vk6c4bsn)

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
public class DoWhileExample {
    public static void main(String[] args) {
      int i=1;
      do {
        System.out.println(i);
        i++;
      } while (i<=5);
    }
}
```

#### Check result [here](https://onecompiler.com/java/3vk6cdxsu)

## For Each

For each loop is used exclusively to loop through elements in an array. Usually for each loop is preferred when you want just values only in less amount of code.

### Syntax

```java
for (type variableName : arrayName) {
  // code 
} 
```
### Example

```java
public class ForExample {
    public static void main(String[] args) {
        String[] cars = {"Volvo", "BMW", "Ford", "Mazda"};
        for (String car : cars) {
            System.out.println(car);
        }
    }
}
```

#### Check Result [here](https://onecompiler.com/java/42mma34yh)
