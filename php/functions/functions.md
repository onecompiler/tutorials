Functions is a sub-routine which contains set of statements. Usually functions are written when multiple calls are required to same set of statements which increases re-usuability and modularity.

Functions allows you to divide your large lines of code into smaller ones. Usually the division happens logically such that each function performs a specific task and is up to developer.

Two types of functions are present in PHP:

1. Built-in Functions

Built-in functions can be called directly by the programmers anywhere in the script. No need to define them as they are already defined and there are more than 1000 built-in functions in PHP.

2. User defined functions

User defined functions are the ones which are written by the programmer based on the requirement. Programmers define them on their own.


## How to define a Function
```php
function functionName() {
  //code
}
```
## How to call a Function

```php
functionName (parameters)
```
# Different ways of calling a user-defined functions

You can call functions in different ways as given below based on the criteria.

1. Function with no arguments and no return value.
2. Function with no arguments and a return value.
3. Function with arguments and no return value.
4. Function with arguments and a return value.

Let's look examples for the above types.

## 1. Function with no arguments and no return value.

```php
<?php
greetings();

function greetings()  
{  
    echo "Hello world!!";  
} 
?> 
```
### check result [here](https://onecompiler.com/php/3vsujd525)

## 2. Function with no arguments and a return value.

```php
<?php
$result = sum();
echo("Sum : $result");
  
function sum() {
   $x = 10;
   $y = 20;
 
   $sum = $x + $y;
   return ($sum); // returning sum value
}
?>
```

### Check result [here](https://onecompiler.com/php/3vsujrxp9)

## 3. Function with arguments and no return value.

```php
<?php

$x= 10;
$y = 20;
sum($x,$y); // passing arguments to function sum

function sum($x , $y) {
   $sum = $x + $y;
   echo("Sum : $sum");
}
?>
```
### check result [here](https://onecompiler.com/php/3vsujgeet)

## 4. Function with arguments and a return value.

```php
<?php
$x= 10;
$y = 20;

$result = sum($x, $y); // passing arguments to function sum
echo("Sum : ". $result);

function sum($x , $y) { //function definition
   $sum = $x + $y;
   return $sum; // returning sum value
}
?>
```
### Check result [here](https://onecompiler.com/php/3vsujyy6w)
