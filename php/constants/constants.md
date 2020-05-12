Constants are like variables with fixed values and they can not be changed once defined. Constants have global scope by default.

# How to create a constant

You can create constants in two ways:

## 1. using define()

`define()` is used to create a constant.

```php
define(name, value, case-insensitive)
```
* **name**: name of the constant
* **value**: value of the constant
* **case-insensitive**: Specifies whether the constant name should be case-insensitive or not. By default it is set to false.

### Example

```php
<?php
define("MESSAGE", "Happy learning!");
echo MESSAGE;
?>
```
### Run [here](https://onecompiler.com/php/3vsr5xpxg)

You can define the constant name as case-insensitive by seeting the third parameter in the baove syntax to true.

```php
<?php
define("MESSAGE", "Happy learning!\n", TRUE);
echo MESSAGE;
echo MesSAGE;
echo message;
?>
```
### Run [here](https://onecompiler.com/php/3vsr6ktcx)


## 2. Usinf const keyword

`const` keyword is used to create a constant. Constants defined with const keyword are case-sensitive.

```php
<?php
const MESSAGE = "Happy learning!";
echo MESSAGE;
?>
```
### Run [here](https://onecompiler.com/php/3vsr9swd2)

