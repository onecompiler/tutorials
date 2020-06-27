Let us understand the below terms before we get into more details.

### 1. Operator

An operator is a symbol which has special meaning and performs an operation on single or multiple operands like addition, substraction etc. In the below example, `+` is the operator. 

```c
package main
import "fmt"
func main() {
     var x, y int
     x = 20
     y = 10
     sum := x + y
     fmt.Println("Sum of two numbers = ",sum)
}
```
### Check result [here](https://onecompiler.com/go/3vppwtvxn)

### 2. Operand

An operand is what operators are applied on. In the above example `x` and `y` are the operands.

# Types of Operators in Go

## 1. Arithmetic Operators

Go arithmetic operators are used to perform arithmetic operations on operands.

|Operator|	Description	| Example|
|----|----|----|
| +	| Used to perform Addition |	8+2 = 10|
| - | Used to perform Subtraction |	12-2 = 10|
| * | Used to perform Multiplication |	5*2 = 10|
| / | Used to perform Division	| 100/10 = 10|
| % | Used to return Remainder	| 40%10 = 0|

`++` and `--` have been removed from operators as they are statements in Go.

### Example

```c
package main
import "fmt"

func main() {
   var x, y, sum, diff, product, division, mod int;
   x = 90;
   y = 10;
 
   sum = x + y;
   fmt.Println("Sum : ", sum);
   
   diff = x - y;
   fmt.Println("Difference : ", diff);
   
   product = x * y;
   fmt.Println("Product : ", product);
   
   division = x / y;
   fmt.Println("Division : ", division);
   
   mod = x % y;
   fmt.Println("Remainder : ", mod);
}
```
### Check Result [here](https://onecompiler.com/go/3vppx8fbm)

## 2. Comparison Operators

Go comparison operators are used to compare two operands. 

| Operator | Description| Usage|
|----|----|----|
| == | Is equal to | x == y|
| != | Not equal to |	!=x |
| > | Greater than | x > y |
| >= | Greater than or equal to |	x >= y|
| < | Less than| x < y |
| <= | Less than or equal to| x <= y|

### Example

```c
package main
import "fmt"

func main() {
  x := 90;
  y := 10;
   
  if ( x == y) {
    fmt.Println("x and y are equal");
  }
  
  if ( x != y) {
    fmt.Println("x and y are not equal");
  }
  
  if ( x > y) {
    fmt.Println("x is greater than y");
  }
  
  if ( x < y) {
    fmt.Println("x is less than y");
  }
}
```
### Check Result [here](https://onecompiler.com/go/3vppy44rf)

## 3. Bitwise Operators

Go bitwise operators are used to perform bitwise operations on operands.

|Operator|	Description| Usage|
|----|----|----|
| & |	Bitwise AND | (x > y) & (y > z)|
| \| |	Bitwise OR | (x > y) \| (y > z)|
| ^ |	Bitwise XOR | (x > y) ^ (y > z)|
| &^ |	Bitwise AND NOT	| (&^x)|
| << | Bitwise Left Shift| x << y|
| >> | Bitwise Right Shift| x >> y|

## 4. Logical operators

Below are the logical operators present in the Go.

|Operator|	Description| Usage|
|----|----|----|
| && |	Logical AND | (x > y) && (y > z)|
| \|\| |	Logical OR | (x > y) \|\| (y > z)|
| ! |	Logical NOT	| (!x)|

## 5. Assignment Operators

Below are the assignment operators present in the Go.

|Operator|	Description| Usage|
|----|----|----|
| =	| Assign| int x = 10;|
| += |	Add and assign|	int x=10; x+=30; // x becomes 40|
| -= |	Subtract and assign| int x=40; x-=10; // x becomes 30|
| *= |	Multiply and assign| int x=10; x*=40; // x becomes 400|
| /= |	Divide and assign|	int x=100; x /= 10;// x becomes 10|
| %= |	Modulus and assign|	int x=100; x%=10; // x becomes 0|
| &= | Bitwise and assign| x &= 10 is same as x = x & 10|
| ^= | Bitwise exclusive OR and assign| x ^= 10 is same as x = x ^ 10|
| \|= |Bitwise inclusive OR and assign	| x \|= 10 is same as x = x \| 10|

### Example

```c
package main
import "fmt"

func main() {
var x int = 10; // assigning 10 to x 
fmt.Println("x value: " , x);
        
x+=30;
fmt.Println("x value after += operation: " , x);
        
x-=10;
fmt.Println("x value after -= operation: " , x);
        
x*=10;
fmt.Println("x value after *= operation: " , x);
        
x/=10;
fmt.Println("x value after /= operation: " , x);
        
x%=10;
fmt.Println("x value after %= operation: " , x);   
}

```

### Check Result [here](https://onecompiler.com/go/3vppym7fz)

