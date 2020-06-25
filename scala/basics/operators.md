An operator is a symbol which has special meaning and performs an operation on single or multiple operands like addition, substraction etc. Scala provides rich set of in-built operators.

# Types of Operators in Scala

## 1. Arithmetic Operators

Arithmetic operators are used to perform arithmetic operations on operands.

|Operator|	Description	| Example|
|----|----|----|
| +	| Used to perform Addition |	8+2 = 10|
| - | Used to perform Subtraction |	12-2 = 10|
| * | Used to perform Multiplication |	5*2 = 10|
| / | Used to perform Division and returns float values	| 10/3 = 3.3333|
| % | Used to return Remainder	| 40%10 = 0|


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


## 3. Bitwise Operators

Bitwise operators are used to perform bitwise operations on operands.

|Operator|	Description| Usage|
|----|----|----|
| & |	Bitwise AND | (x > y) & (y > z)|
| \| |	Bitwise OR | (x > y) \| (y > z)|
| ^ |	Bitwise XOR | (x > y) ^ (y > z)|
| ~ |	Bitwise NOT	| (~x)|
| << | Bitwise Left Shift| x << y|
| >> | Bitwise Right Shift| x >> y|
| >>> | Shift right zero fill operator | x >>> y |

## 4. Logical Operators

Logical operators are as shown below:

|Operator|	Description| Usage|
|----|----|----|
| && |	Logical AND | (x > y) && (y > z)|
| \|\| |	Logical OR | (x > y) \|\| (y > z)|
| ! |	Logical NOT	| (!x)|

## 5. Assignment Operators

Assignment operators are as shown below:

|Operator|	Description| Usage|
|----|----|----|
| =	| Assign| x = 10;|
| += |	Add and assign| x=10; x+=30; //x becomes 40|
| -= |	Subtract and assign| x=40; x-=10; //x becomes 30|
| *= |	Multiply and assign|  x=10; x*=40; //x becomes 400|
| /= |	Divide and assign|	 x=100; x /= 10; //x becomes 10|
| %= |	Modulus and assign|	 x=100; x%=10;  //x becomes 0|
| <<= | Bitwise Left Shift and assign| x <<= 2; // x = x << 2|
| >>= | Bitwise Right Shift and assign| x >>= 2; // x = x >> 2|
| &= |	Bitwise AND  and assign| x &= y; // x = x & y|
| \|= |	Bitwise OR  and assign| x \|= y; // x = x \| y|
| ^= |	Bitwise XOR  and assign | x ^= y; // x = x ^ y|


## Summary

|Type|Operators|
|----|----|
| Arithmetic Operators| + , - , * , / , %  |
| Comparision Operators| == , != , > , >= , < , <= |
| Bitwise Operators| & , ^ , \| , ^ , ~ , << , >> , >>>|
| Logical Operators| && , \|\| , !|
| Assignment Operators|= , += , -= , *= , /= , %= , &= , ^= , \|= , <<= , >>= |

