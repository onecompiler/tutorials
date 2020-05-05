When ever you want to perform a set of operations based on a condition(s) If / If-Else / Nested ifs are used.

You can also use if-else , nested IFs and IF-ELSE-IF ladder when multiple conditions are to be performed on a single variable.

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
  int x = 30;
  int y = 30;

  if ( x == y) {
    println "x and y are equal";
  }

```
#### Check result [here](https://onecompiler.com/groovy/3vmstfxxj)

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
  int x = 30;
  int y = 20;

  if ( x == y) {
    println "x and y are equal";
  } else {
    println "x and y are not equal";  
  }
```
#### Check result [here](https://onecompiler.com/groovy/3vmstjqra)

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
int age = 15;

    if ( age <= 1 && age >= 0) {
      println "Infant";
    } else if (age > 1 && age <= 3) {
        println "Toddler";
    } else if (age > 3 && age <= 9) {
        println "Child";
    } else if (age > 9 && age <= 18) {
        println "Teen";
    } else if (age > 18) {
        println "Adult";
    } else {
        println "Invalid Age";
    }
```
#### Check result [here](https://onecompiler.com/groovy/3vmstpxqc)

## 4. Nested-If

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
 int age = 50;
 char resident = 'Y';
 if (age > 18)
  {
    if(resident == 'Y'){
      println "Eligible to Vote";
    }
  }
```
#### Check result [here](https://onecompiler.com/groovy/3vmstygr5)

## 5. Switch

Switch is an alternative to IF-ELSE-IF ladder and to select one among many blocks of code.

### Syntax

```c
switch(conditional-expression){    
case value1:    
 // code    
 break;  //optional  
case value2:    
 // code    
 break;  //optional  
...    
    
default:     
//code to be executed when all the above cases are not matched;    
} 
```
### Example
```java
int day = 3;
      
switch(day){
  case 1: println "Sunday";
          break;
  case 2: println "Monday";
          break;
  case 3: println "Tuesday";
          break;
  case 4: println "Wednesday";
          break;
  case 5: println "Thursday";
          break;
  case 6: println "Friday";
          break;
  case 7: println "Saturday";
          break;
  default:println "Invalid day";
          break; 
}
```
####  Check Result [here](https://onecompiler.com/groovy/3vmsu4w3j)
