Data Frame is a data object which has like a 2D array like structure where column contains value of a variable and row contains one set of values from each column.

Data Frame is a list which is of equal length vectors. Data frames can contain different types of data where as Matrix can have only single type of data.

* Column names should be non-empty.
* Row names should be unique.
* Data in a data frame can contain different types of data like a factor, numeric, or character type.
* Every column will have same number of data items.

`frame()` function is used to create a data frame.

## Example
```py
student.data <- data.frame(
   studentID = c (101:104), 
   firstName = c("Foo","Bar","Alex","Mark"),
   marksPercentage = c(97.2,79.2,53.9,87.3), 
   
   joiningDate = as.Date(c("2010-06-01", "2010-06-10", "2010-06-04", "2010-06-02")),
   stringsAsFactors = FALSE
)
print(student.data)
```

### Check Result [here](https://onecompiler.com/r/3vsf3ubwf)

## How to access a Data frame

Data from a data frame can be accessed as shown below:

```py

student.data <- data.frame(
   studentID = c (101:104), 
   firstName = c("Foo","Bar","Alex","Mark"),
   marksPercentage = c(97.2,79.2,53.9,87.3), 
   
   joiningDate = as.Date(c("2010-06-01", "2010-06-10", "2010-06-04", "2010-06-02")),
   stringsAsFactors = FALSE
)
col.data <- data.frame(student.data$firstName,student.data$marksPercentage) # printing specific columns
print(col.data)

row.data <- student.data[1:2] # printing first two rows
print(row.data)

ele.data <- student.data[c(1,3),c(2,4)] #printing an element based on it's row and column
print(ele.data)
```

### Check Result [here](https://onecompiler.com/r/3vsf4bshn)


