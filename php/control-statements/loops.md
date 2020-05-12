## 1. For

For loop is used to iterate a set of statements based on a condition. Usually for loop is preferred when number of iterations are known in advance.

### Syntax

```java
for(Initialization; Condition; Increment/decrement){  
//code  
} 
```
### Example

```php

# print 1 to 10 numbers in php using for loop
<?php
  for ($i = 1; $i <= 10; $i++) {
    echo("$i\n");
  }
?>
```

### Check Result [here](https://onecompiler.com/php/3vsrjtx3e)

## 2. Foreach

Foreach loop is used to loop through each element or key/value pair in an array. It works only on arrays.

### Syntax

```php
foreach ($array as $value) {
  //code 
}
```
### Example
```php
<?php
$directions = array("East", "West", "North", "South");

foreach ($directions as $value) {
  echo "$value \n";
}
?>
```
### Check Result [here](https://onecompiler.com/php/3vsrmfwte)

## 3. While

While is also used to iterate a set of statements based on a condition. Usually while is preferred when number of iterations are not known in advance.

### Syntax

```java
while(condition){  
//code 
}  
```
### Example

```php
# print 1 to 10 numbers in php using while loop
<?php
   $i=1;
   while ( $i <= 10) {
     echo("$i\n");
     $i++;
   }
?>
```
### Check result [here](https://onecompiler.com/php/3vsrk94ke)

## 4. Do-while

Do-while is also used to iterate a set of statements based on a condition. It is mostly used when you need to execute the statements atleast once.

### Syntax

```java
do{  
//code 
}while(condition); 
```
### Example

```php
// print 1 to 10 numbers in php using do-while loop
<?php
  $i=1;
  do {
    echo("$i\n");
    $i++;
  } while ($i<=10);
?>
```
### Check result [here](https://onecompiler.com/php/3vsrkf9ym)