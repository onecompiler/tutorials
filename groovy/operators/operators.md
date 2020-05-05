Let us understand the below terms before we get into more details.

### 1. Operator

An operator is a symbol which has special meaning and performs an operation on single or multiple operands like addition, substraction etc. In the below example, `+` is the operator. 

```java
def x, y, sum;
x = 10;
y = 20;
 
sum = x + y;
println "Sum is: " +sum;
```
### Check result [here](https://onecompiler.com/groovy/3vmpwekxb)

### 2. Operand

An operand is what operators are applied on. In the above example `x` and `y` are the operands.

# Types of Operators in Groovy

## 1. Arithmetic Operators

Arithmetic operators are used to perform arithmetic operations on operands.

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

```java
def x, y, sum, diff, product, division, mod, inc, dec;
x = 90;
y = 10;
 
 sum = x + y;
println "Sum : " + sum ;
   
diff = x - y;
println "Difference : " + diff ;
   
product = x * y;
println "Product : " + product;
   
division = x / y;
println "Division : " + division;
   
mod = x % y;
println "Remainder : " + mod ;
   
inc = x++;
println "x value after incrementing : " + x;
   
dec = x--;
println "x value after decrementing : " + x;
```
### Check Result [here](https://onecompiler.com/groovy/3vmspxnnq)

## 2. Relational Operators

Relational operators are used to compare two operands. 

| Operator | Description| Usage|
|----|----|----|
| == | Is equal to | x == y|
| != | Not equal to |	!=x |
| > | Greater than | x > y |
| >= | Greater than or equal to |	x >= y|
| < | Less than| x < y |
| <= | Less than or equal to| x <= y|

### Example

```java
   int x = 90;
   int y = 10;
   
  if ( x == y) 
    println "x and y are equal";
  
  if ( x != y) 
    println "x and y are not equal";
  
  if ( x > y) 
    println "x is greater than y";
  
  if ( x < y) 
    println "x is less than y";

```
### Check Result [here](https://onecompiler.com/groovy/3vmsqbuup)

## 3. Bitwise Operators

Bitwise operators are used to perform bitwise operations on operands.

|Operator|	Description| Usage|
|----|----|----|
| & |	Bitwise AND | (x > y) & (y > z)|
| \| |	Bitwise OR | (x > y) | (y > z)|
| ^ |	Bitwise XOR | (x > y) ^ (y > z)|
| ~ |	Bitwise NOT	| (~x)|

## 4. Logical operators

Below are the logical operators present in Groovy.

|Operator|	Description| Usage|
|----|----|----|
| && |	Logical AND | (x > y) && (y > z)|
| \|\| |	Logical OR | (x > y) \|\| (y > z)|
| ! |	Logical NOT	| (!x)|

## 5. Assignment Operators

Below are the assignment operators present in Groovy.

|Operator|	Description| Usage|
|----|----|----|
| =	| Assign| int x = 10;|
| += |	Add and assign|	int x=10; x+=30; // x becomes 40|
| -= |	Subtract and assign| int x=40; x-=10; // x becomes 30|
| *= |	Multiply and assign| int x=10; x*=40; // x becomes 400|
| /= |	Divide and assign|	int x=100; x /= 10;// x becomes 10|
| %= |	Modulus and assign|	int x=100; x%=10; // x becomes 0|

### Example

```java
int x = 10; // assigning 10 to x 
println "x value: " + x ;
        
x+=30;
println "x value after += operation: " + x ;
        
x-=10;
println "x value after -= operation: " + x ;
        
x*=10;
println"x value after *= operation: " + x ;
        
x/=10;
println"x value after /= operation: " + x ;
        
x%=10;
println"x value after %= operation: " + x;   
```

### Check Result [here](https://onecompiler.com/groovy/3vmsqqzv7)

## Summary

| Operator type | Description|
|----|-----|
| Arithmetic Operators|+ , - , * , / , %|
| comparision Operators| < , > , <= , >=, != , ==| 
| Bitwise Operators| & , ^ , \|| 
| Logical Operators| && , \|\|, ! |
| Assignment Operators|= , += , -= , *= , /= , %=|
