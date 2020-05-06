Let us understand the below terms before we get into more details.

### 1. Operator

An operator is a symbol which has special meaning and performs an operation on single or multiple operands like addition, substraction etc. In the below example, `+` is the operator. 

```javascript
let x = 2, y= 3;
let sum = x + y;
console.log(sum); // prints 5
```
### 2. Operand

An operand is what operators are applied on. In the above example `x` and `y` are the operands.

### 3. Unary

If the operator is applied on a single operand then it is called unary. In the below example, `+` doesn't change the value of x. `+` operator will not do anything to numbers but converts strings or boolean values to numbers. This is equal to  `Number()` method.

```javascript
let x = -4;
console.log(+x); // There will be no effect on numbers;

let y= true;
console.log(y); // prints true
console.log(+y); // prints 1
```

```javascript
let x = 4;
x = -x; // Unary nagation 
console.log(x); // prints -4
```

### 4. Binary

If the operator is applied on a two operands then it is called binary.


```javascript
let x = 2, y= 3;
let multiply = x * y; // binary * operator
console.log(multiply); // prints 6
```

### 5. Ternary 

If the operator is applied on a three operands then it is called ternary. This is also known as conditional operator as a condition is followed by `?` and true-expression which is followed by a `:` and false expression. This is oftenly used as a shortcut to replace if-else statement

```javascript
let x = 2, y= 3;
let z = x > y ? x : y; // z will be assigned with x if x is greater than y and viceversa
console.log(z); // prints 3
```

If we need to write the same logic using If-else, the code looks bigger with more statements. Hence ternary operator is used as a shortcut for these kind of scenarios.

```javascript
let x = 2, y= 3;
let z;

if ( x > y) {
    z = x;
} else {
    z = y;
}; 
console.log(z); // prints 3
```

# Operators in Javascript

## 1. Arithmetic Operators

JavaScript arithmetic operators are used to perform arithmetic operations on operands.

|Operator|	Description	| Example|
|----|----|----|
| +	| Used to perform Addition |	8+2 = 10|
| - | Used to perform Subtraction |	12-2 = 10|
| * | Used to perform Multiplication |	5*2 = 10|
| / | Used to perform Division	| 100/10 = 10|
| % | Used to perform Remainder	| 40%10 = 0|
| ++ | Used to perform Increment |	let a=10; a++; // a becomes 11|
| -- | Used to perform Decrement |	let a=10; a--; // a becomes 9|

## 2. Comparison Operators

JavaScript comparison operators are used to compare two operands. 

| Operator | Description| Usage|
|----|----|----|
| == | Is equal to | x == y|
| === |	Same as is equal to | x === y|
| != | Not equal to |	!=x |
| !== |	Not Identical to |	x!==x|
| > | Greater than | x > y |
| >= | Greater than or equal to |	x >= y|
| < | Less than| x < y |
| <= | Less than or equal to| x <= y|

## 3. Bitwise Operators

JavaScript bitwise operators are used to perform bitwise operations on operands.

|Operator|	Description| Usage|
|----|----|----|
| & |	Bitwise AND | (x > y) & (y > z)|
| \| |	Bitwise OR | (x > y) \| (y > z)|
| ^ |	Bitwise XOR | (x > y) ^ (y > z)|
| ~ |	Bitwise NOT	| (~x)|
| << | Bitwise Left Shift| x << y|
| >> | Bitwise Right Shift| x >> y|
| >>> | Bitwise Right Shift with Zero| x >>> y|


## 4. Logical operators

Below are the logical operators present in the Javascript.

|Operator|	Description| Usage|
|----|----|----|
| && |	Logical AND | (x > y) && (y > z)|
| \|\| |	Logical OR | (x > y) \|\| (y > z)|
| ! |	Logical NOT	| (!x)|

## 5. Assignment Operators

Below are the assignment operators present in the Javascript.

|Operator|	Description| Usage|
|----|----|----|
| =	| Assign| let x = 10;|
| += |	Add and assign|	let x=10; x+=30; // x becomes 40|
| -= |	Subtract and assign| let x=40; x-=10; // x becomes 30|
| *= |	Multiply and assign| let x=10; x*=40; // x becomes 400|
| /= |	Divide and assign|	let x=100; x /= 10;// x becomes 10|
| %= |	Modulus and assign|	let x=100; x%=10; // x becomes 0|

## 6. Special Operators

Below are the special operators present in javascript.


| Operator| Description| 
|----|----|
| ?: |	Conditional or ternary Operator, shortcut to if-else|
| , |	Allows multiple expressions to be evaluated as single statement.|
|new|	creates an instance |
|typeof|	checks which type of object.|
|void|	used to discards the return value of an expression.|
|yield|	used to check what is returned in a generator by the generator's iterator.|
|delete| to delete a property from the object.
|in| used to check if object has the given property|
|instanceof| used to check if the object is an instance of given type|


