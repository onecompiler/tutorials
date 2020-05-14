In PHP, there is no need to explicitly declare variables to reserve memory space. When you assign a value to a variable, declaration happens automatically. Variables are case-sensitive in PHP. Variable names starts with `$` sign.

```php
$variable-name = value;  
```

## Naming convention of variables in PHP

* Variable names starts with `$` followed by name of the variable.
* variable name should begin with a `letter` or underscore(`_`).
* Variables naming cannot contain white spaces like `first name` which is a invalid variable name.
* variable name cannot start with a number.
* variable names are case-sensitive and hence `name`, `NAME` and `Name` are three different variables.

## Example

```php
<?php
$str = "Hello World!";
echo "$str";
?>
```

# Scope of the variables

In PHP, Variables can be declared anywhere in the script and it's scope will be decided based on where it is declared.

There are three different scopes of variables:

1. Global
2. Local
3. Static

## 1. Global

A variable declared outside a function will have GLOBAL SCOPE and they can only be accessed outside the function. A global variable can be accessed inside a function using `global` keyword before the variables. 

## 2. Local

A variable declared inside a function will have LOCAL SCOPE and they can only be accessed inside the function.

## 3. Static

Usually variables which are declared inside a function will get deleted after the function completion. If you want to retain the local variable information which should be available for next function execution, then you must declare them as static variables.

### Examples

```php
<?php
$x = 20; // global scope variable

function sum() {
  global $x;  // if you don't declare them as global, then you will get Undefined variable error
  $y = 30; //local variable
  static $z = 1; // static variable
  
  $sum = $x + $y;
  echo("z: $z \n"); // z value gets incremented every time function gets called
  $z++; 
  
  return $sum;
}

$result = sum();
echo ("Sum:$result\n"); 

$result = sum();
echo ("Sum:$result\n"); 

$result = sum();
echo ("Sum:$result\n"); 

?>
```
### Run [here](https://onecompiler.com/php/3vsnr5ctd)
