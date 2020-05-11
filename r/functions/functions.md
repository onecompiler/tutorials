Function is a sub-routine which contains set of statements. Usually functions are written when multiple calls are required to same set of statements which increases re-usuability and modularity.

Functions allows you to divide your large lines of code into smaller ones. Usually the division happens logically such that each function performs a specific task and is up to developer.

R provides a large number of in-built functions (refer [here](https://cran.r-project.org/doc/contrib/Short-refcard.pdf)) and users also can create their own functions.

## How to define a Function

```py
function_name <- function(arg1, arg2, ...) {
  #code 
}
```

## How to call a Function

```c
function_name(arguments)
```
## Examples

### 1. Function with no arguments

```py
greetings <- function() {
  print("I'm in function")
}

greetings()
```
### Check Result [here](https://onecompiler.com/r/3vs9yu5yk)

### 2. Function with arguments

```py
sum <- function(x, y) {
  cat("Sum:", x+y)
}

sum(10, 20)
```
### Check Result [here](https://onecompiler.com/r/3vs9yxzy2)
