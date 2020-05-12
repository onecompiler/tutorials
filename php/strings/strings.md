
* String is a sequence of characters.
* Strings are used to store text data.
* Strings are declared using double quotes. 

### Example
```php
$name = "OneCompiler";
```
# String Functions

Some of the popular string functions are given below with examples

## 1. strlen()

`strlen()` function is used to return the length of a string.

```php
<?php
  echo strlen("One Compiler"); 
?>
```
### Run [here](https://onecompiler.com/php/3vsrrqgrn)

## 2. str_word_count()

`str_word_count()` function is used to count the number of words present in a string.

```php
<?php
echo str_word_count("Hello world! Happy Learning!!"); 
?>
```
### Run [here](https://onecompiler.com/php/3vsrru8x4)

## 3. strrev() 

`strrev()` function is used to reverse a  given string.

```php
<?php
  echo strrev("One Compiler"); 
?>
```
### Run [here](https://onecompiler.com/php/3vsrryrnx)

## 4. str_replace() 

`str_replace()` function is used to replace one or more characters with other given characters in a string.

```php
<?php
echo str_replace("bar", "Foo", "Good morning bar!"); 
?>
```
### Run [here](https://onecompiler.com/php/3vsrscrh7)

## 5. strpos()

`strpos()` function is used to search for a specific text within a string. If the text is found, then it returns the character position of the first match. Else, it will return FALSE.

```php
<?php
  echo strpos("Hello world! Happy Learning!!", "appy"); 
?>
```
### Run [here](https://onecompiler.com/php/3vsrryrnx)
