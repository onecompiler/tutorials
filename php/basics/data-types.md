As the name suggests, data-type specifies the type of the data present in the variable. Variables need not to be declared with a data-type in PHP as it is a loosely typed language. Below are the different data types in PHP.

## 1. Integer data type 

`int` is used to store whole numbers from -2147483648 to 2147483647.

```php
<?php
$x = 999999;
echo($x);
?>
```

## 2. String data type 

`string` is a sequence of characters.

```php
<?php
$str = "Good morning..";
echo($str);
?>
```

## 3. Float data type 

`Float` is used represent decimal values.

```php
<?php
$x = 79.22;
echo($x);
?>
```

## 4. Boolean data type 

`Boolean` data type is used only when the value is either true or false. 

```php
<?php
$isAvailable = TRUE;
echo($isAvailable);
?>
```

## 5. Array data type 

`Array` is used store multiple values in a single variable. They can contain mixed type values as well. `var_dump()` is used to return data type along with it's value. Let's learn more in detail about arrays in next chapters.

```php
<?php
$arr = array("iPhone", 1000, TRUE);
var_dump($arr);
?>
```
## 6. Object data type 

`Object` data type is used to store data and information on how to process the data. For declaring object, you must explicitly declare class and objects. Let's learn more in detail about objects in next chapters.

## 7. Null data type 

`Null` data type is a special data type which can store only `Null` value.
```php
<?php
$str = "OneCompiler";
$str = null;
var_dump($str);
?>
```

## 8. Resource data type

Resource data type is used to store references to resources which are external to PHP and hence these are not actual data types. Example of a resource data type is database connections.
