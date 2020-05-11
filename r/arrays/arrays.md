Array is a data object which allows users to store data in more than two dimensions. `array()` function is used to create an array in R.

## How to create an array

### Syntax

```java
arrayName <- array(data, dim= (rowSize, columnSize, matrices, dimNames))  
```
* **data** : data is an input vector
* **rowSize** : defines no of row elements array can store
* **columnSize** : defines no of column elements array can store
* **dimNames** : specifies row and column names
* **matrices** : array can consists of multi-dimensional matrices

### Example

```py
x <- c(1,2,3)`
y <- c(4,5,6,7,8,9)

arr <- array(c(x,y),dim=c(3,3,2)) # 3 rows, 3 columns and 2 matrices
print(arr)
```
### Check Result [here](https://onecompiler.com/r/3vsckfmd3)

Below is an example which shows you how to name rows, columns and matrices.

```py
x <- c(1,2,3)
y <- c(4,5,6,7,8,9)

row.names <- c("row-1","row-2","row-3")
col.names <- c("col-1","col-2","col-3")
matrix.names <- c("Matrix-1","Matrix-2","matrix-3")
arr <- array(c(x,y),dim=c(3,3,3), dimnames = list(row.names, col.names, matrix.names)) # 3 rows, 3 columns and 3 matrices
print(arr)
```
### Check Result [here](https://onecompiler.com/r/3vsckmhr3)

## How to access Array elements

Array elements are accessed by using it's indices.

```py
x <- c(1,2,3)
y <- c(4,5,6,7,8,9)

row.names <- c("row-1","row-2","row-3")
col.names <- c("col-1","col-2","col-3")
matrix.names <- c("Matrix-1","Matrix-2","matrix-3")
arr <- array(c(x,y),dim=c(3,3,3), dimnames = list(row.names, col.names, matrix.names)) # 3 rows, 3 columns and 3 matrices
print(arr)

print(arr[,,3]) # prints 3rd matrix

print(arr[2,,2]) # prints 2nd row in second matrix

print(arr[3,3,1]) # prints 3rd row 3rd column element of 1st matrix
```
### Check Result [here](https://onecompiler.com/r/3vsckzatj)

