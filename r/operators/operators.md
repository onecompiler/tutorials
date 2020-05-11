Operator is a symbol which specifies an action. R provides rich built-in operators.

# Types of Operators in R

## 1. Arithmetic Operators

R arithmetic operators are used to perform arithmetic operations on operands.

|Operator|	Description	| 
|----|----|
| +	| Used to perform Addition |	
| - | Used to perform Subtraction |
| * | Used to perform Multiplication |	
| / | Used to perform Division	| 
| %% | Used to return Remainder	| 
| ^ | used to perform Exponentiation | 

### Example

```java
x <- c (5, 2, 2)
y <- c (7, 3, 9)
cat ("x + y:", x + y, "\n\n")

x <- c (5, 2, 2)
y <- c (7, 3, 9)
cat ("y - x:", y - x, "\n\n")

x <- c (5, 2, 2)
y <- c (7, 3, 9)
cat ("x * y:", x * y, "\n\n")

x <- c (5, 2, 2)
y <- c (7, 3, 9)
cat ("y/x:",  y/x , "\n\n")

x <- c (5, 2, 2)
y <- c (7, 3, 9)
cat ("x %% y :", x %% y, "\n\n")

x <- c (5, 2, 2)
y <- c (7, 3, 9)
cat ("x ^ y:", x ^ y, "\n\n")

```
### Check Result [here](https://onecompiler.com/r/3vs93kjrv)

## 2. Relational Operators

R relational operators are used to compare two operands. 

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
x <- c (5, 2, 9)
y <- c (7, 2, 3)

cat("Is x > y: ", x > y, "\n\n")

cat("Is x < y: ", x < y, "\n\n")

cat("Is x >= y: ", x >= y, "\n\n")

cat("Is x <= y: ", x <= y, "\n\n")

cat("Is x != y: ", x != y, "\n\n")

cat("Is x <= y: ", x == y, "\n\n")

```
### Check Result [here](https://onecompiler.com/r/3vs9a3nam)

## 3. Logical operators

Below are the logical operators present in the R.

|Operator|	Description| 
|----|----|
| & |	Element wise Logical AND | 
| \| |	Element wise Logical OR | 
| ! |	Logical NOT	| 
| && |	Logical AND | 
| \|\| |	Logical OR | 

### Example

```java
x <- c(7, 1, 0, 0, TRUE, 3+2i)
y <- c(9, 1, 0, 1, FALSE, 2+3i)

cat("x&y:", x&y, "\n\n") 

cat("x|y:", x|y, "\n\n")

cat("!x:", !x, "\n\n")

cat("x&&y:", x&&y, "\n\n") # gives result only for the first element of both vectors

cat("x||y:", x||y, "\n\n")# gives result only for the first element of both vectors

```
### Check Result [here](https://onecompiler.com/r/3vs9gqzy8)

## 4. Assignment Operators

Below are the assignment operators present in R.

|Operator|	Description| 
|----|----|
| <- or = or <<-	|  left assignment operators| 
| 	-> or ->> |	 right assignment operators|	

### Example

```java

x <- c(7, 1, 0, 0, TRUE, 3+2i)
y <<- c(9, 1, 0, 1, FALSE, 2+3i)
z = c(5, 2, 3, TRUE)

cat("x:",x,"\n\n")

cat("y:",y,"\n\n")

cat("z:",z,"\n\n")


c(7, 2, 0, 0, TRUE, 5+2i) -> a
c(9, 2, 0, 1, FALSE, 2+3i) ->> b

cat("a:",a,"\n\n")

cat("b:",b,"\n\n")
```

### Check Result [here](https://onecompiler.com/r/3vs9hj93z)

## 5. Miscellaneous  Operator
| Operator type | Description|
|----|-----|
|:|	used to create a series of numbers in sequence for a given vector|
|%in%|	used to check if an element belongs to a vector and returns true or false|	
|%*%|	used to multiply a matrix with its transpose|	

### Example

```java
x <- 1:10
cat("x:", x, "\n\n")

y <- 7
z <- 19
cat("Is y is in x: ", y %in% x, "\n\n")
cat("Is z is in x: ", z %in% x, "\n\n")

X = matrix( c(1,2,3,4,5,6), nrow = 2,ncol = 3,byrow = TRUE)
m = X %*% t(X)
print(m)
```
### Check Result [here](https://onecompiler.com/r/3vs9jfr3k)

## Summary

| Operator type | Description|
|----|-----|
| Arithmetic Operator|+ , - , * , / , %%, ^|
| Relational Operator| < , > , <= , >=, != , ==| 
| Logical Operator| &, \|, && , \|\|, ! |
| Assignment Operator|= , ->, ->>, <-, <<- |
| Misc Operators| :, %in%, %*%|

