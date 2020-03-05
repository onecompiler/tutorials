# If Family

When ever you want to perform a set of operations based on a condition(s) IF/IF-ELSE/Nested ifs are used.

You can also use if-else for nested IFs and IF-ELSE-IF ladder when multiple conditions are to be performed on a single variable.

## If

### Syntax

```java
if(conditional-expression)
{
    //code
}
```
### Example

```java
public class Test {

    public static void main(String[] args) {
        
        int x = 30;
        int y = 30;

        if ( x == y) {
        System.out.println("x and y are equal");
        }
    }
}
```
#### Check result [here](https://onecompiler.com/java/3vk67ygka)

## If-else

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
public class Test {

    public static void main(String[] args) {
        
        int x = 30;
        int y = 20;

        if ( x == y) {
          System.out.println("x and y are equal");
        } else {
          System.out.println("x and y are not equal");  
        }
    }
}
```
#### Check result [here](https://onecompiler.com/java/3vk67mrw4)

## If-else-if ladder

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
public class Test {

    public static void main(String[] args) {
        
        int age = 15;

        if ( age <= 1 && age >= 0) {
          System.out.println("Infant");
        } else if (age > 1 && age <= 3) {
          System.out.println("Toddler");
        } else if (age > 3 && age <= 9) {
          System.out.println("Child");
        } else if (age > 9 && age <= 18) {
          System.out.println("Teen");
        } else if (age > 18) {
          System.out.println("Adult");
        } else {
          System.out.println("Invalid Age");
        }
    }
}
```
#### Check result [here](https://onecompiler.com/java/3vk683h7e)

## Nested-If

Nested-Ifs represents if block within another if block. 

### Syntax
```java
if(conditional-expression-1) {    
     //code    
          if(conditional-expression-2) {  
             //code
             if(conditional-expression-3) {
                 //code
             }  
    }    
}
```

### Example
```java
public class Test {

    public static void main(String[] args) {
        
        int age = 50;
        String resident = "yes";
        
        if (age > 18)
        {
          if(resident == "yes"){
            System.out.println("Eligible to Vote");
          }
        }

    }
}
```
#### Check result [here](https://onecompiler.com/java/3vk683h7e)

## SWITCH

Switch is an alternative to IF-ELSE-IF ladder and to select one among many blocks of code.

### Syntax

```java
switch(conditional-expression){    
case value1:    
 //code    
 break;  //optional  
case value2:    
 //code    
 break;  //optional  
...    
    
default:     
 code to be executed when all the above cases are not matched;    
} 
```
### Example
```java
public class SwitchExample {
    public static void main(String[] args) {
      int day = 3;
      
      switch(day){
        case 1: System.out.println("Sunday");
        break;
        case 2: System.out.println("Monday");
        break;
        case 3: System.out.println("Tuesday");
        break;
        case 4: System.out.println("Wednesday");
        break;
        case 5: System.out.println("Thursday");
        break;
        case 6: System.out.println("Friday");
        break;
        case 7: System.out.println("Saturday");
        break;
        default:System.out.println("Invalid day");
        break; 
      }
    }
}
```
####  Check Result [here](https://onecompiler.com/java/3vk6b38qb)
