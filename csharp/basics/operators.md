
An operator is a symbol which has special meaning and performs an operation on single or multiple operands like addition, substraction etc. C# provides rich set of in-built operators.

# Types of Operators in C#

## 1. Arithmetic Operators

C# arithmetic operators are used to perform arithmetic operations on operands.

|Operator|	Description	| Example|
|----|----|----|
| +	| Used to perform Addition |	8+2 = 10|
| - | Used to perform Subtraction |	12-2 = 10|
| * | Used to perform Multiplication |	5*2 = 10|
| / | Used to perform Division	| 100/10 = 10|
| % | Used to return Remainder	| 40%10 = 0|
| ++ | Used to perform Increment |	int a=10; a++; // a becomes 11|
| -- | Used to perform Decrement |	int a=10; a--; // a becomes 9|

### Example

```c#
using System;

namespace Operators
{
	public class ArithmeticOperators
	{
		public static void Main(string[] args)
		{
	    int x, y, sum, diff, product, division, mod, inc, dec;
      x = 90;
      y = 10;
 
      sum = x + y;
      Console.WriteLine("Sum : {0}" ,sum);
   	  
   	  diff = x - y;
   	  Console.WriteLine( "Difference : {0}" , diff );
   
   	  product = x * y;
   	  Console.WriteLine( "Product : {0}" , product );
   
   	  division = x / y;
   	  Console.WriteLine( "Division : {0}" , division );
   
   	  mod = x % y;
   	  Console.WriteLine( "Remainder : {0}" , mod );
   
   	  inc = x++;
   	  Console.WriteLine( "x value after incrementing : {0}" , x );
   
     	dec = x--;
    	Console.WriteLine( "x value after decrementing : {0}" , x);
 
 	}
	}
}

```
### Check Result [here](https://onecompiler.com/csharp/3vt2t4m8v)

## 2. Relational Operators

C# relational operators are used to compare two operands. 

| Operator | Description| Usage|
|----|----|----|
| == | Is equal to | x == y|
| != | Not equal to |	!=x |
| > | Greater than | x > y |
| >= | Greater than or equal to |	x >= y|
| < | Less than| x < y |
| <= | Less than or equal to| x <= y|

### Example

```C#
using System;

namespace Operators
{
	public class LogicalOperators
	{
		public static void Main(string[] args)
		{
		  
      int x = 90;
      int y = 10;
   
      if ( x == y) {
        Console.WriteLine( "x and y are equal" );
      }
  
      if ( x != y) {
        Console.WriteLine( "x and y are not equal" );
      }
  
      if ( x > y) {
        Console.WriteLine( "x is greater than y" );
      }
  
      if ( x < y) {
        Console.WriteLine( "x is less than y" );
      }

	}
	}
}
```
### Check Result [here](https://onecompiler.com/csharp/3vt2vyftr)

## 3. Bitwise Operators

C# bitwise operators are used to perform bitwise operations on operands.

|Operator|	Description| Usage|
|----|----|----|
| & |	Bitwise AND | (x > y) & (y > z)|
| \| |	Bitwise OR | (x > y) \| (y > z)|
| ^ |	Bitwise XOR | (x > y) ^ (y > z)|
| ~ |	Bitwise NOT	| (~x)|
| << | Bitwise Left Shift| x << y|
| >> | Bitwise Right Shift| x >> y|

## 4. Logical operators

Below are the logical operators present in the C#.

|Operator|	Description| Usage|
|----|----|----|
| && |	Logical AND | (x > y) && (y > z)|
| \|\| |	Logical OR | (x > y) \|\| (y > z)|
| ! |	Logical NOT	| (!x)|

## 5. Assignment Operators

Below are the assignment operators present in the C#.

|Operator|	Description| Usage|
|----|----|----|
| =	| Assign| int x = 10;|
| += |	Add and assign|	int x=10; x+=30; // x becomes 40|
| -= |	Subtract and assign| int x=40; x-=10; // x becomes 30|
| *= |	Multiply and assign| int x=10; x*=40; // x becomes 400|
| /= |	Divide and assign|	int x=100; x /= 10;// x becomes 10|
| %= |	Modulus and assign|	int x=100; x%=10; // x becomes 0|
| <<= | Left shift and assign|	x <<= 2 is same as x = x << 2|
| >>= | Right shift and assign|	x >>= 2 is same as x = x >> 2|
| &= | Bitwise and assign| x &= 10 is same as x = x & 10|
| ^= | Bitwise exclusive OR and assign| x ^= 10 is same as x = x ^ 10|
| \|= |Bitwise inclusive OR and assign	| x \|= 10 is same as x = x \| 10|

## 6. Misc Operator

* Ternary Operator

If the operator is applied on a three operands then it is called ternary. This is also known as conditional operator as a condition is followed by `?` and true-expression which is followed by a `:` and false expression. This is oftenly used as a shortcut to replace if-else statement

### Example

```c#
using System;

namespace Operators
{
	public class TernaryOperators
	{
		public static void Main(string[] args)
		{
		  
    int x = 10;
    int y = 90;

    int z = x > y ? x : y;

    Console.WriteLine("Larger Number is: {0}", z);

	}
	}
}
```
### Check Result [here](https://onecompiler.com/csharp/3vt2w7mct)

* sizeof()

This operator is used to return the size of a variable.

```c#
using System;

namespace Operators
{
	public class Operators
	{
		public static void Main(string[] args)
		{
		  
        Console.WriteLine("Size of x is: {0}", sizeof(int));
        Console.WriteLine("Size of x is: {0}", sizeof(double));
	
	}
	}
}
```
### Check Result [here](https://onecompiler.com/csharp/3vt2we4vm)

## Summary

| Operator type | Description|
|----|-----|
| Arithmetic Operator|+ , - , * , / , %|
| comparision Operator| < , > , <= , >=, != , ==| 
| Bitwise Operator| & , ^ , \|, ^ , ~, <<, >> |
| Logical Operator| && , \|\|, ! |
| Assignment Operator|= , += , -= , *= , /= , %=, <<=, >>=, &=, ^=, \|= |
| Ternary Operator (? :)| conditional Operator, x > y ? x : y. if condition is true executes x else y |
| sizeof ()| Used to return the size of a data type |
| typeof()|	Used to return the type of a class.|
| & | used to return the address of an variable.|
| * |	Pointer to a variable.|
| is | Used to check whether an object is of a certain type.|
| as | Used to cast without raising an exception if the cast fails.|


