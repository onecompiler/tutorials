
An operator is a symbol which has special meaning and performs an operation on single or multiple operands like addition, substraction etc. Ruby provides rich set of in-built operators.

# Types of Operators in Ruby

## 1. Arithmetic Operators

Arithmetic operators are used to perform arithmetic operations on operands.

|Operator|	Description	| Example|
|----|----|----|
| +	| Used to perform Addition |	8+2 = 10|
| - | Used to perform Subtraction |	12-2 = 10|
| * | Used to perform Multiplication |	5*2 = 10|
| / | Used to perform Division and returns float values	| 10/3 = 3.3333|
| % | Used to return Remainder	| 40%10 = 0|
| ** | Used to raise power of another operand| x ** y = x to the power of y|


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
| <=>| combined comparision operator | x <=> y|
| === | equality operator | (1...5) === 3|
| .eql?| equality operator, returns true one if both are of same type and equal values | x.eql?y|
| equal? | equality operator, returns true one if both are of same object ID | x.equal?y|

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


## 4. Logical Operators

Logical operators are as shown below:

|Operator|	Description| Usage|
|----|----|----|
| && |	Logical AND | (x > y) && (y > z)|
| \|\| |	Logical OR | (x > y) \|\| (y > z)|
| ! |	Logical NOT	| (!x)|
| and |	Logical AND | (x > y) and (y > z)|
| or |	Logical OR | (x > y) or (y > z)|
| not |	Logical NOT	| not(x && y)|

## 5. Assignment Operators

Assignment operators are as shown below:

|Operator|	Description| Usage|
|----|----|----|
| =	| Assign| x = 10;|
| += |	Add and assign| x=10; x+=30; #x becomes 40|
| -= |	Subtract and assign| x=40; x-=10; #x becomes 30|
| *= |	Multiply and assign|  x=10; x*=40; #x becomes 400|
| /= |	Divide and assign|	 x=100; x /= 10; #x becomes 10|
| %= |	Modulus and assign|	 x=100; x%=10;  #x becomes 0|
| **= | exponential calculation and assign| `x **= y is equivalent to x = x**y`|

## 6. Ternary Operator

If the operator is applied on a three operands then it is called ternary. This is also known as conditional operator as a condition is followed by `?` and true-expression which is followed by a `:` and false expression. This is oftenly used as a shortcut to replace if-else statement

|Operator|	Description| Usage|
|----|----|----|
|? : |	Conditional Expression	| (x > y) ? (code if x > y) : (code if x < y)

## 7. Range Operators

Below are the range operators:

|Operator|	Description| Usage|
|----|----|----|
| ..| Creates a range from start to end including end range|  1..10, creates a range from 1 to 10|
| ...| Creates a range from start to end excluding end range|  1...10, creates a range from 1 to 9|

## 8. Special Operators

|Operator|	Description| Usage|
|----|----|----|
| defined? |It is a special operator which is like a method call, returns a description string of the expression if defined else nil | defined? variable or a method call|
| dot(.) | Used to refer a  method present in a module | modulename.method|
| :: | Used to refer constants, instance methods and class methods defined within a class or module, to be accessed from anywhere outside the class or module | Modulename::const|

## Summary

|Type|Operators|
|----|----|
| Arithmetic Operators| + , - , * , / , % , ** |
| Comparision Operators| == , != , > , >= , < , <= , <=>, .eql?, ===, equal? |
| Bitwise Operators| & , ^ , \| , ^ , ~ , << , >> |
| Logical Operators| && , \|\| , ! , and , or, not|
| Assignment Operators|= , += , -= , *= , /= , %= , **= |
| Ternary Operators | ? :|
| Range Operators | .. , ...|
| Special Operators | defined?, . , :: |
