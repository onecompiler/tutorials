Let us understand the below terms before we get into more details.

### 1. Operator

An operator is a symbol which has special meaning and performs an operation on single or multiple operands like addition, substraction etc. In the below example, `+` is the operator. 

```py
x = 1
y = 2
print (x+y)
```
### 2. Operand

An operand is what operators are applied on. In the above example `x` and `y` are the operands.

Python provides the below operator groups:

## 1. Arithmetic Operators

Python arithmetic operators are used to perform arithmetic operations on operands.

|Operator|	Description	| Example|
|----|----|----|
| +	| Used to perform Addition |	8+2 = 10|
| - | Used to perform Subtraction |	12-2 = 10|
| * | Used to perform Multiplication |	5*2 = 10|
| / | Used to perform Division	| 100/10 = 10|
| % | Used to perform Remainder	| 40%10 = 0|
| ** | Used to perform Exponentiation |	10 ** 2 |
| // | Used to perform Floor division |	9 // 4 |

## 2. Comparison Operators

Python comparison operators are used to compare two operands. 

| Operator | Description| Usage|
|----|----|----|
| == | Is equal to | x == y|
| != | Not equal to |	!=x |
| > | Greater than | x > y |
| >= | Greater than or equal to |	x >= y|
| < | Less than| x < y |
| <= | Less than or equal to| x <= y|

## 3. Bitwise Operators

Python bitwise operators are used to perform bitwise operations on operands.

|Operator|	Description| Usage|
|----|----|----|
| & |	Bitwise AND | (x > y) & (y > z)|
| \| |	Bitwise OR | (x > y) \| (y > z)|
| ^ |	Bitwise XOR | (x > y) ^ (y > z)|
| ~ |	Bitwise NOT	| (~x)|
| << | Bitwise Left Shift| x << y|
| >> | Bitwise Right Shift| x >> y|


## 4. Logical Operators

Below are the logical operators present in Python.

|Operator|	Description| Usage|
|----|----|----|
| && |	Logical AND | (x > y) && (y > z)|
| \|\| |	Logical OR | (x > y) \|\| (y > z)|
| ! |	Logical NOT	| (!x)|

## 5. Assignment Operators

Below are the assignment operators present in Python.

|Operator|	Description| Usage|
|----|----|----|
| =	| Assign| x = 10;|
| += |	Add and assign| x=10; x+=30; #x becomes 40|
| -= |	Subtract and assign| x=40; x-=10; #x becomes 30|
| *= |	Multiply and assign|  x=10; x*=40; #x becomes 400|
| /= |	Divide and assign|	 x=100; x /= 10; #x becomes 10|
| %= |	Modulus and assign|	 x=100; x%=10;  #x becomes 0|
| **= | exponential calculation and assign| `x **= y is equivalent to x = x**y`|
| //= | floor division and assign | x //= y is equivalent = x= x // y|

## 6. Membership Operators

Membership operators are used to test  if a sequence is present in a given object.

|Operator|	Description| Usage|
|----|----|----|
|in  | Returns True if a sequence is present in the object | x in y |
|not in | Returns True if a sequence is not present in the object | x not in y|

## 7. Identity Operators

Identity operators are used to compare the objects whether they same objects with the same memory location.

|Operator|	Description| Usage|
|----|----|----|
|is  | Returns true if both variables are actually same objects| x is y 	|
|is not | Returns true if both variables are not the same objects |	x is not y|

## Summary

|Type|Operators|
|----|----|
| Arithmetic Operators| +  -  *  /  %  **  //|
| Comparision Operators| ==  !=   >  >=  <  <=|
| Bitwise Operators| &  ^  \| ^ ~ << >> |
| Logical Operators| &&  \|\| ! |
| Assignment Operators|=  +=  -=  *=  /=  %=  **=  //=|
| Membership Operators| in, not in |
| Identity Operators | is, is not|

