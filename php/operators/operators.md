Let us understand the below terms before we get into more details.

### 1. Operator

An operator is a symbol which has special meaning and performs an operation on single or multiple operands like addition, substraction etc. In the below example, `+` is the operator. 

```php
<?php
$x = 10;
$y = 20;
$sum = $x + $y;
echo($sum);
?>
```
### Check result [here](https://onecompiler.com/php/3vsnugjc5)

### 2. Operand

An operand is what operators are applied on. In the above example `x` and `y` are the operands.

# Types of Operators in PHP

## 1. Arithmetic Operators

Arithmetic operators are used to perform arithmetic operations on operands.

|Operator|	Description	| Example|
|----|----|----|
| +	| Used to perform Addition |	8+2 = 10|
| - | Used to perform Subtraction |	12-2 = 10|
| * | Used to perform Multiplication |	5*2 = 10|
| / | Used to perform Division	| 100/10 = 10|
| % | Used to return Remainder	| 40%10 = 0|
| ++ | Used to perform Increment |	$a=10; $a++; // a becomes 11|
| -- | Used to perform Decrement |	$a=10; $a--; // a becomes 9|


### Example

```php
<?php
	$x = 90;
  $y = 10;

  $sum = $x + $y;
  echo("Sum of two numbers: $sum\n");
  $diff = $x - $y;
  echo("Difference between two numbers: $diff\n");
  $multiply = $x * $y;
  echo("Product of two numbers: $multiply\n");
  $div = $x / $y;
  echo("Division of two numbers: $div\n");
  $mod = $x % $y;
  echo("Modulus of two numbers: $mod\n");
  $x++;
  echo("x value after incrementing :$x\n");
  $x--;
  echo("x value after decrementing :$x\n");
?>
```
### Check Result [here](https://onecompiler.com/php/3vsnxmae2)

## 2. Comparison Operators

Comparison operators are used to compare two operands. 

| Operator | Description| Usage|
|----|----|----|
| == | Is equal to | x == y|
| != | Not equal to |	!=x |
| > | Greater than | x > y |
| >= | Greater than or equal to |	x >= y|
| < | Less than| x < y |
| <= | Less than or equal to| x <= y|

### Example

```php
<?php
  $x = 90;
  $y = 10;

  if ( $x == $y) {
  echo("$x and $y are equal\n");
  }
  if ( $x != $y) {
  echo("$x and $y are not equal\n");
  }
  if ( $x > $y) {
  echo("$x is greater than $y\n");
  }
  if ( $x < $y) {
  echo("$x is less than $y\n");
  }
?>
```
### Check Result [here](https://onecompiler.com/php/3vsny23ep)

## 3. Logical operators

Below are the logical operators present in the Php.

|Operator|	Description| Usage|
|----|----|----|
| && |	Logical AND | (x > y) && (y > z)|
| \|\| |	Logical OR | (x > y) \|\| (y > z)|
| ! |	Logical NOT	| (!x)|
| and |	Logical AND | (x and y)|
| or | |	Logical OR | (x or y) |

## 4. Assignment Operators

Below are the assignment operators present in the Php.

|Operator|	Description| Usage|
|----|----|----|
| =	| Assign| int x = 10;|
| += |	Add and assign|	int x=10; x+=30; // x becomes 40|
| -= |	Subtract and assign| int x=40; x-=10; // x becomes 30|
| *= |	Multiply and assign| int x=10; x*=40; // x becomes 400|
| /= |	Divide and assign|	int x=100; x /= 10;// x becomes 10|
| %= |	Modulus and assign|	int x=100; x%=10; // x becomes 0|

### Example

```php
<?php

  $x = 10; // assigning 10 to $x 
  echo("x value: $x\n");
  
  $x+=30;
  echo("x value after += operation:  $x\n");
  
  $x-=10;
  echo("x value after -= operation:  $x\n");
  
  $x*=10;
  echo("x value after *= operation:  $x\n");
  
  $x/=10;
  echo("x value after /= operation:  $x\n");
  
  $x%=10;
  echo("x value after %= operation:  $x\n");
?>
```

### Check Result [here](https://onecompiler.com/php/3vsnye44d)


## 5. Conditional Operator

If the operator is applied on three operands then it is called conditional operator. This is also known as ternary operator as a condition is followed by `?` and true-expression which is followed by a `:` and false expression. This is oftenly used as a shortcut to replace if-else statement.

### Example

```php
<?php
  $x = 10;
  $y = 50;
  $largerNumber = ($x > $y) ? $x : $y;
  echo("Larger number is :$largerNumber");
?>
```
### Check Result [here](https://onecompiler.com/php/3vsnynd6q)

## Summary

| Operator type | Description|
|----|-----|
| Arithmetic Operator|+ , - , * , / , %, ++, --|
| comparision Operator| < , > , <= , >=, != , ==| 
| Logical Operator| && , \|\|, ! |
| Assignment Operator|= , += , -= , *= , /= , %= |
| Conditional Operator| ? : |

