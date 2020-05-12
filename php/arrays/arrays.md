An array can store multiple values in a single variable.

You can declare an array by using `array()` function.

## Syntax

```php
$arrName = array(values..);
```

There are three types of arrays:

## 1. Indexed arrays

Elements of the indexed arrays are accessed using it's indices. Their index starts from `0`.

There are two ways to create indexed arrays as shown below:

```php
$directions = array("East", "West", "North", "South");
```
or assign manually

```php
$directions[0] = "East";
$directions[1] = "West";
$directions[2] = "North";
$directions[3] = "South";
```
### Example
```php
<?php
$directions = array("East", "West", "North", "South");
$length = count($directions);

for($i=0; $i < $length; $i++) {
  echo "$directions[$i]\n";
}
?>
```
### Run [here](https://onecompiler.com/php/3vsrpxtvn)

## 2. Associative arrays

Associative arrays are the arrays which contain key-value pairs.

There are two ways to create asspciative arrays as shown below:

```php
$capitals = array("Japan" => "Tokyo", "India" => "New Delhi","United Kingdom" => "London","United States" => "Washington, D.C.","China" => "Beijing");
```
or assign manually

```php
$capitals["Japan"] = "Tokyo";
$capitals["India"] = "New Delhi";
$capitals["United Kingdom"] = "London";
$capitals["United States"] = "Washington, D.C.";
$capitals["China"] = "Beijing";
```

### Example
```php
<?php
$capitals = array("Japan" => "Tokyo", "India" => "New Delhi","United Kingdom" => "London","United States" => "Washington, D.C.","China" => "Beijing");
foreach($capitals as $key => $value) {
  echo "capital of ". $key . " is " . $value. "\n";
}
?>
```
### Run [here](https://onecompiler.com/php/3vsrqpjby)

## 3. Multi dimensional arrays

A multidimensional array is an array which contain one or more arrays.

### Two-dimensional array

You can create a two-dimensional array as shown below:
```php
$num = array(
  array(1,2,3),
  array(4,5,6),
  array(7,8,9)
  );
```
### Example 
```php
<?php
$num = array(
  array(1,2,3),
  array(4,5,6),
  array(7,8,9)
  );
  for ($i = 0; $i < 3; $i++){
    for ($j=0; $j <3; $j++){
      echo($num[$i][$j]);
      echo("\t");
    }
    echo("\n");
  }
?>
```
### Run [here](https://onecompiler.com/php/3vsrr5x6s)

