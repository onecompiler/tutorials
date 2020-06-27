
An operator is a symbol which has special meaning and performs an operation on single or multiple operands like addition, substraction etc. VB.net provides rich set of in-built operators.

# Types of Operators in VB.net

## 1. Arithmetic Operators

Arithmetic operators are used to perform arithmetic operations on operands.

|Operator|	Description	| Example|
|----|----|----|
| +	| Used to perform Addition |	8+2 = 10|
| - | Used to perform Subtraction |	12-2 = 10|
| * | Used to perform Multiplication |	5*2 = 10|
| / | Used to perform Division and returns float values	| 10/3 = 3.3333|
| \ | Used to perform Division and returns integer	| 100\10 = 10|
| MOD | Used to return Remainder	| 40 MOD 10 = 0|
| ^ | Used to raise power of another operand| x ^ y = x to the power of y|

### Example

```c#
Public Module Program
	Public Sub Main(args() As string)
        Dim x As Integer = 10
        Dim y As Integer = 2
        Dim sum As Integer 
        Dim diff As Integer 
        Dim product As Integer 
        Dim div As Integer 
        Dim mods As Integer 
        Dim power As Integer 
        
        sum = x + y
        Console.WriteLine("x + y: {0} ", sum)

        diff = x - y
        Console.WriteLine("x - y: {0} ", diff)

        product = x * y
        Console.WriteLine("x * y: {0} ", product)

        div = x / y
        Console.WriteLine("x / y : {0}", div)

        mods = x Mod y
        Console.WriteLine("x mod y: {0}", mods)

        power = x ^ y
        Console.WriteLine("x ^ y : {0}", power)

    End Sub
End Module
```
### Check Result [here](https://onecompiler.com/vb/3vtqxspy9)

## 2. Relational Operators

VB.net relational operators are used to compare two operands. 

| Operator | Description| Usage|
|----|----|----|
| = | Is equal to | x == y|
| > | Greater than | x > y |
| >= | Greater than or equal to |	x >= y|
| < | Less than| x < y |
| <= | Less than or equal to| x <= y|

## 3. Bitwise Operators

VB.net bitwise operators are used to perform bitwise operations on operands.

|Operator|	Description| 
|----|----|
| AND |	Bitwise AND | 
| OR |	Bitwise OR | 
| XOR |	Bitwise XOR | 
| NOT |	Bitwise NOT	| 
| AndAlso | Logical AND|
| OrElse| Logical OR|
| isFalse| checks if given expression is false|
| isTrue| checks if given expression is true|

## 4. Bitwise Shift Operators

VB.net bitwise operators are used to perform bitwise operations on operands.

|Operator|	Description| 
|----|----|
| And |	Binary AND | 
| Or |	Binary OR | 
| Xor |	Binary XOR | 
| Not |	Binary NOT	| 
| << | Binary Left Shift Operator|
| >> | Binary Right Shift Operator|

## 4. Logical operators

Below are the logical operators present in the VB.net.

|Operator|	Description| Usage|
|----|----|----|
| && |	Logical AND | (x > y) && (y > z)|
| \|\| |	Logical OR | (x > y) \|\| (y > z)|
| ! |	Logical NOT	| (!x)|

## 5. Assignment Operators

Below are the assignment operators present in the VB.net.

|Operator|	Description| Usage|
|----|----|----|
| =	| Assign| int x = 10;|
| += |	Add and assign|	int x=10; x+=30; // x becomes 40|
| -= |	Subtract and assign| int x=40; x-=10; // x becomes 30|
| *= |	Multiply and assign| int x=10; x*=40; // x becomes 400|
| /= |	Divide and assign|	int x=100; x /= 10;// x becomes 10|
| \= |	Divide and assign|	int x=100; x /= 10;// x becomes 10|
| %= |	Modulus and assign|	int x=100; x%=10; // x becomes 0|
| <<= | Left shift and assign|	x <<= 2 is same as x = x << 2|
| >>= | Right shift and assign|	x >>= 2 is same as x = x >> 2|
| &= | concatenation and assign| x &= 10 is same as x = x & 10|
| ^= | Exponentiation and assign| x ^= 10 is same as x = x ^ 10|

## 6. Miscellaneous Operators

| Operator | Description|
|----|----|
|AddressOf | Returns the address|
| Await | applied to an operand while executing an asynchronous method |
| GetType | returns type of a given object|


