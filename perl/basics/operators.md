Let us understand the below terms before we get into more details.

### 1. Operator

An operator is a symbol which has special meaning and performs an operation on single or multiple operands like addition, substraction etc. In the below example, `+` is the operator. 

```perl
$x = 10;
$y = 20;
$sum = $x + $y;
print($sum);
```
### Check result [here](https://onecompiler.com/perl/3vnhpvaqe)

### 2. Operand

An operand is what operators are applied on. In the above example `x` and `y` are the operands.

# Types of Operators in Perl

## 1. Arithmetic Operators

Perl arithmetic operators are used to perform arithmetic operations on operands.

|Operator|	Description	| Example|
|----|----|----|
| +	| Used to perform Addition |	8+2 = 10|
| - | Used to perform Subtraction |	12-2 = 10|
| * | Used to perform Multiplication |	5*2 = 10|
| / | Used to perform Division	| 100/10 = 10|
| % | Used to return Remainder	| 40%10 = 0|
| ** | Used to perform Exponentation |	10**2 means 10 to the power of 2|

### Example

```perl
   $x = 90;
   $y = 10;
 
   $sum = $x + $y;
   print ("Sum : " , $sum ,"\n");
   
   $diff = $x - $y;
   print ( "Difference : " , $diff  ,"\n");
   
   $product = $x * $y;
   print ( "Product : " , $product  ,"\n");
   
   $division = $x / $y;
   print ( "Division : " , $division  ,"\n");
   
   $mod = $x % $y;
   print ( "Remainder : " , $mod  ,"\n");
   
   $inc = $x++;
   print ( "x value after incrementing : " , $x  ,"\n");
   
   $dec = $x--;
   print ( "x value after decrementing : " , $x ,"\n");
 
   $pow = $y ** 2;
   print ( "$y to the power of 2 : " , $pow  ,"\n");
   
```
### Check Result [here](https://onecompiler.com/perl/3vnqjre9n)

## 2. Equality Operators

Equality operators are used to compare two operands. 

| Operator | Description| Usage|
|----|----|----|
| == | Is equal to | x == y|
| != | Not equal to |	!=x |
| > | Greater than | x > y |
| >= | Greater than or equal to |	x >= y|
| < | Less than| x < y |
| <= | Less than or equal to| x <= y|

### Example

```perl
   $x = 90;
   $y = 10;
   
  if ( $x == $y) {
    print "x and y are equal \n";
  }
  
  if ( $x != $y) {
    print "x and y are not equal \n";
  }
  
  if ( $x > $y) {
    print "x is greater than y \n";
  }
  
  if ( $x < $y) {
    print "x is less than y \n";
  }

```
### Check Result [here](https://onecompiler.com/perl/3vnqk9rur)

## 3. Bitwise Operators

Bitwise operators are used to perform bitwise operations on operands.

|Operator| Description| Usage|
|----|----|----|
| `&` |	Bitwise AND | `($x > $y) & ($y > $z)`|
| `|` |	Bitwise OR | `($x > $y) | ($y > $z)`|
| `^` |	Bitwise $xOR | `($x > $y) ^ ($y > $z)`|
| `~` |	Bitwise NOT	| `(~$x)`|
| `<<` | Bitwise Left Shift| `$x << $y`|
| `>>` | Bitwise Right Shift| `$x >> $y`|

## 4. Logical operators

Below are the logical operators present in Perl.

| Operator| Description| Usage|
|----|----|----|
| `&&` | Logical AND | `($x > $y) && ($y > $z)`|
| `||` | Logical OR | `($x > $y) || ($y > $z)`|
| `!` |	Logical NOT	| `!($x)`|

## 5. Assignment Operators

Below are the assignment operators present in Perl.

|Operator|	Description| Usage|
|----|----|----|
| = | Assign| $x = 10;|
| += |	Add and assign|	$x=10; $x+=30; # x becomes 40|
| -= |	Subtract and assign| $x=40; x-=10; # x becomes 30|
| *= |	Multiply and assign| $x=10; x*=40; # x becomes 400|
| /= |	Divide and assign|	$x=100; $x /= 10;# x becomes 10|
| %= |	Modulus and assign|	$x=100; $x%=10; # x becomes 0|
| <<= | Left shift and assign|	$x <<= 2 is same as $x = $x << 2|
| >>= | Right shift and assign|	$x >>= 2 is same as $x = $x >> 2|
| &= | Bitwise and assign| $x &= 10 is same as $x = $x & 10|
| ^= | Bitwise exclusive OR and assign| $x ^= 10 is same as $x = $x ^ 10|
| \|= |Bitwise inclusive OR and assign	| `$x |= 10 is same as $x = $x | 10`|

### Example

```perl
$x = 10; #assigning 10 to x 
print "x value: $x \n ";
        
$x+=30;
print "x value after += operation: $x \n ";
        
$x-=10;
print "x value after -= operation: $x \n ";
        
$x*=10;
print "x value after *= operation: $x \n ";
        
$x/=10;
print "x value after /= operation: $x \n ";
        
$x%=10;
print "x value after %= operation: $x";   
```

### Check Result [here](https://onecompiler.com/perl/3vnqmhza4)

## 6. Quote like operators

|Operator|	Description| Example |
|----|----|----|
|q{ } | Encloses the given string within single quotes | q{One Compiler} is same as 'One Compiler'|
|qq{ }| Encloses the given string within double quotes | qq{One Compiler} is same as "One Compiler"|
|qx{ }| Encloses the given string within invert quotes | qx{One Compiler} is same as `One Compiler`|
