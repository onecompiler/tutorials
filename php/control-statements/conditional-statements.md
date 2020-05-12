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

```php
<?php
  $x = 30;
  $y = 30;

  if ( $x == $y) {
    echo("x and y are equal");
  }
?>
```
### Check result [here](https://onecompiler.com/php/3vsra5pxs)

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

```php
<?php
  $x = 30;
  $y = 60;

  if ( $x == $y) {
    echo("x and y are equal");
  } else {
      echo("x and y are not equal");
  }
?>
```
### Check result [here](https://onecompiler.com/php/3vsra9tw9)

## 3. If-else-if 

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
```php
<?php
 $age = 15;

 if ( $age <= 1 && $age >= 0) {
   echo("Infant");
 } else if ($age > 1 && $age <= 3) {
   echo("Toddler");
 } else if ($age > 3 && $age <= 9) {
   echo("Child");
 } else if ($age > 9 && $age <= 18) {
   echo("Teen");
 } else if ($age > 18) {
   echo("Adult");
 } else {
   echo("Invalid $age");
 }

?>
```
### Check result [here](https://onecompiler.com/php/3vsrageyk)

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
```php
<?php
  $age = 50;
  $resident = "yes";
        
  if ($age > 18)
  {
    if($resident == "yes"){
      echo("Eligible to Vote");
    }
  }
?>
```
### Check result [here](https://onecompiler.com/php/3vsrak3fc)

## 5. Switch

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
 //code to be executed when all the above cases are not matched;    
} 
```
### Example
```php
<?php
  $day = 3;
      
  switch($day){
   case 1: echo("Sunday");
   break;
   case 2: echo("Monday");
   break;
   case 3: echo("Tuesday");
   break;
   case 4: echo("Wednesday");
   break;
   case 5: echo("Thursday");
   break;
   case 6: echo("Friday");
   break;
   case 7: echo("Saturday");
   break;
   default:echo("Invalid day");
   break; 
  }

?>
```
###  Check Result [here](https://onecompiler.com/php/3vsrarrvg)
