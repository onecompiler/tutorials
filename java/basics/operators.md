Let us understand the below terms before we get into more details.

### 1. Operator

An operator is a symbol which has special meaning and performs an operation on single or multiple operands like addition, substraction etc. In the below example, `+` is the operator. 

```java
public class SumOfTwoNumbers {

    public static void main(String[] args) {
        
        int x = 10;
        int y = 90;

        int sum = x + y;

        System.out.println("The sum is: " + sum);
    }
}
```
#### Check result [here](https://onecompiler.com/java/3vjya75bd)

### 2. Operand

An operand is what operators are applied on. In the above example `x` and `y` are the operands.

# Types of Operators in Java

## 1. Arithmetic Operators

Java arithmetic operators are used to perform arithmetic operations on operands.

|Operator|	Description	| Example|
|----|----|----|
| +	| Used to perform Addition |	8+2 = 10|
| - | Used to perform Subtraction |	12-2 = 10|
| * | Used to perform Multiplication |	5*2 = 10|
| / | Used to perform Division	| 100/10 = 10|
| % | Used to return Remainder	| 40%10 = 0|

### Example

```java
public class ArithmeticOperators {

    public static void main(String[] args) {
        
        int x = 90;
        int y = 10;

        int sum = x + y;
        System.out.println("Sum of two numbers: " + sum);
        int diff = x - y;
        System.out.println("Difference between two numbers: " + diff);
        int multiply = x * y;
        System.out.println("Product of two numbers: " + multiply);
        int div = x / y;
        System.out.println("Division of two numbers: " + div);
        int mod = x % y;
        System.out.println("Modulus of two numbers: " + mod);
    }
}
```
#### Check Result [here](https://onecompiler.com/java/3vjycvhr6)

## 2. Comparison Operators

Java comparison operators are used to compare two operands. 

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
public class ComparisionOperators {

    public static void main(String[] args) {
        
        int x = 90;
        int y = 10;

        if ( x == y) {
        System.out.println("x and y are equal");
        }
        if ( x != y) {
        System.out.println("x and y are not equal");
        }
        if ( x > y) {
        System.out.println("x is greater than y");
        }
        if ( x < y) {
        System.out.println("x is less than y");
        }
    }
}
```
#### Check Result [here](https://onecompiler.com/java/3vjye43a3)

## 3. Bitwise Operators

Java bitwise operators are used to perform bitwise operations on operands.

|Operator|	Description| Usage|
|----|----|----|
| `&` |	Bitwise AND | `(x > y) & (y > z)`|
| `|` |	Bitwise OR | `(x > y) | (y > z)`|
| `^` |	Bitwise XOR | `(x > y) ^ (y > z)`|
| `~` |	Bitwise NOT	| `(~x)`|
| `<<` | Bitwise Left Shift| `x << y`|
| `>>` | Bitwise Right Shift| `x >> y`|

## 4. Logical operators

Below are the logical operators present in the Java.

|Operator|	Description| Usage|
|----|----|----|
| `&&` |	Logical AND | `(x > y) && (y > z)`|
| `||` |	Logical OR | `(x > y) || (y > z)`|
| `!` |	Logical NOT	| `(!x)`|

## 5. Assignment Operators

Below are the assignment operators present in the Java.

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
public class assignmentOperators {

    public static void main(String[] args) {
        
        int x = 10; // assigning 10 to x 
        System.out.println("x value: " + x);
        
        x+=30;
        System.out.println("x value after += operation: " + x);
        
        x-=10;
        System.out.println("x value after -= operation: " + x);
        
        x*=10;
        System.out.println("x value after *= operation: " + x);
        
        x/=10;
        System.out.println("x value after /= operation: " + x);
        
        x%=10;
        System.out.println("x value after %= operation: " + x);
        
    }
}
```

#### Check Result [here](https://onecompiler.com/java/3vk2w9xvp)

## 6. Auto-increment and Auto-decrement Operators

Below are the Auto-increment and Auto-decrement Operators in Java.

|Operator|	Description	| Example|
|----|----|----|
| ++ | Used to perform Increment |	int a=10; a++; // a becomes 11|
| -- | Used to perform Decrement |	int a=10; a--; // a becomes 9|

### Example
```java
public class ShiftOperators {

    public static void main(String[] args) {
        
        int x = 10;
        
        int y = x++;
        int z = x--;
        System.out.println("x value after ++ operation: " + y);
        
        System.out.println("x value after -- operation: "+ z);
    }
}
```
#### Check Result [here](https://onecompiler.com/java/3vk2txd3r)

## 7. Ternary Operator

If the operator is applied on a three operands then it is called ternary. This is also known as conditional operator as a condition is followed by `?` and true-expression which is followed by a `:` and false expression. This is oftenly used as a shortcut to replace if-else statement

### Example

```java
public class TernaryOperator {

    public static void main(String[] args) {
        
        int x = 10;
        int y = 90;

        int z = x > y ? x : y;

        System.out.println("Larger Number is: " + z);
    }
}
```
#### Check Result [here](https://onecompiler.com/java/3vjycn8eu)

## Summary

| Operator type | Description|
|----|-----|
| Arithmetic Operator|+ , - , * , / , %|
| comparision Operator| < , > , <= , >=, != , ==| 
| Bitwise Operator| & , ^ , | 
| Logical Operator| && , `||`, ! |
| Assignment Operator|= , += , -= , *= , /= , %= |
| Auto-increment and Auto-decrement Operators| ++ , -- |
| Ternary Operator| ? : |

