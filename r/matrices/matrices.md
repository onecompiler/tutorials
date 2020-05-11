Matrix is a two-dimensional rectangular data set. Elements of a matric will be of same atomic type. You can create a character or logical matrix but we dont them much. Numeric matrices are very popular for mathematical calculations.

```py
matrix(data, rowSize, columnSize, byrow, dimnames)
```
* **data** : data is an input vector
* **rowSize** : defines no of row elements array can store
* **columnSize** : defines no of column elements array can store
* **byrow** : If it is set to TRUE then the input vector elements are arranged by row.
* **dimNames** : specifies row and column names

```py
x <- c(1:9)
mrx <- matrix(x, nrow = 3, byrow = TRUE) #arranging elements by row
print(mrx)

x <- c(1:9)
mrx <- matrix(x, nrow = 3, byrow = FALSE) #arranging elements by column
print(mrx)

#arranging elements by row and with row and column names
row.names <- c("row-1","row-2","row-3")
col.names <- c("col-1","col-2","col-3")
mrx <- matrix(x, nrow = 3, byrow = TRUE, dimnames =list(row.names,col.names))
print(mrx)
```
### Check Result [here](https://onecompiler.com/r/3vseyh2jq)

## How to access elements of matrix

Matrix's elements are using by it's row and column indices.

```py
x <- c(1:9)

#defining row and column names
row.names <- c("row-1","row-2","row-3")
col.names <- c("col-1","col-2","col-3")
mrx <- matrix(x, nrow = 3, byrow = TRUE, dimnames =list(row.names,col.names))
print(mrx)

print(mrx[,3]) # prints 3rd column

print(mrx[1,]) # prints 1st row

print(mrx[3,3]) # prints 3rd row 3rd column element 
```

### Check Result [here](https://onecompiler.com/r/3vseyxynw)
