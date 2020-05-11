
## 1. If
If is used when we want to execute a code only if a certain condition is satisfied.

### Syntax

```py
if(conditional-expression)
{
    # code
}
```
### Example

```py
x <- 30
y <- 30

if ( x == y ) {
   print("x and y are equal")
}
```
### Check result [here](https://onecompiler.com/r/3vs9sb43n)

## 2. If-else

If-else is used when we want to execute one block of code among different blocks of code based on a given condition.

### Syntax

```py
if(conditional-expression)
{
    # code
} else {
    # code
}
```
### Example

```py
x <- 30
y <- 50

if ( x > y ) {
   print("x is greater than y")
} else {
  print("y is greater than x")
}
```
### Check result [here](https://onecompiler.com/r/3vs9smpp7)

## 3. Switch

Switch is an alternative to IF-ELSE-IF ladder and to select one among many blocks of code.

### Syntax


```py
switch(expression, case-1, case-2, case-3....)   
```

### Example
```py
day <- switch(5 ,
"Sunday",
"Monday",
"Tuesday",
"Wednesday",
"Thursday",
"Friday",  
"Saturday")

print(day)
```
####  Check Result [here](https://onecompiler.com/r/3vs9tejeu)
