Vectors are considered as basic R objects. Vectors are classified into two types.

1. Atomic Vectors

    There are six types of atomic vectors in R.

    * logical
    * integer
    * double
    * complex
    * character 
    * raw

2. Lists
Let's discuss abour Lists in next chapters

# How to create vectors

## 1. Single Element Vector

When you assign just one value to a vector then it is called single element vector

### Example
```java
x <- "s" #Character type atomic vector
print(x); 

x <- 53.2 #double type atomic vector
print(x)

x <- 79L #integer type atomic vector
print(x)

x <- TRUE # Logical type atomic vector
print(x)

x <- 2+5i# complex type atomic vector
print(x)

x <- charToRaw('OneCompiler') #  raw type atomic vector.
print(x)
```
### Check Result [here](https://onecompiler.com/r/3vsc5vfe4)

## 2. Multiple Elements vector

When you assign more values to a vector then it is called multiple elements vector

### Example

```java
x <- 1:10 # Creating a sequence from 1 to 10 using colon
print(x); 

x <- 5.5:10.5 # Creating a sequence of decimal values using colon
print(x); 

x <- seq(1, 5, by = 0.5) # creating sequence using seq()
print(x)

x <- c('hello', 0, 3, TRUE) # mixed type vector
print(x)

```
### Check Result [here](https://onecompiler.com/r/3vsc7xsse)

# How to access Vectors

Vectors can be accessed using it's indices. Indexing starts with position 1 means `vector[0]` represents it's first element.

```java

 x <- c(1, "One", 2 , "two", 3 , "three", 4 , "four", 5 , "five")
cat("first element:",x[1],"\n\n");
cat("Fifth element:",x[5],"\n\n");
cat("Tenth element:",x[10],"\n\n");
cat("Fifteenth element:",x[15],"\n\n");

```
### Check result [here](https://onecompiler.com/r/3vsc8ey6d)
