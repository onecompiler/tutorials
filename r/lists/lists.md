List is a R-Object which can contain elements of different types such as numbers, vectors, strings or another list inside it. 

List is a generic vector which contains mixed data type elements. 

## How to create a list

`list()` function is used to create a list.  

```py
list()
```

### Example

```py
x <- c(1,2,3,4,5)
y <- c("hello", "happy", "world")
z <- c(TRUE, FALSE, TRUE)

list1 <- list(x,y,z)
```
You can also assign a name to the list elements with the help of `names()` function.

```py
emp_list <- list( c("Foo","Bar", "Alex", "Mark"), c(1,2,3,4))

names(emp_list) <- c("Names","Id")

print(emp_list)
```

### Check Result [here](https://onecompiler.com/r/3vscgpppr)

## How to access elements of a list

You can access elements of a list using two methods
1. Indexing
2. List names

```py
emp_list <- list( c("Foo","Bar", "Alex", "Mark"), c(1,2,3,4))
names(emp_list) <- c("Names","Id")

print(emp_list[1]) # accessing list elements by indexing

print(emp_list["Id"]) # accessing list elements by it's name
```
### Check Result [here](https://onecompiler.com/r/3vscgvg96)
