Usually in other programming languages, you declare variables with some data type to reserve memory space for that variable. 

In R, Variables are not declared with data types like in other programming languages. In R, the variables are assigned with R-Objects and hence the data type of the R-object becomes the data type of the variable. Some of the R-objects are as follows:

* Vectors
* Lists
* Arrays
* Data Frames
* Matrices
* Factors

Let's discuss more in-detail about the above R-objects in coming chapters.

There are six data types of the atomic vectors and usually the R-Objects are built upon the atomic vectors. These are also called as six classes of vectors.

| Data type | Description | Usage |
|----|----|----|
|Numeric|To represent decimal values| x=1.84|
|Integer| To represent integer values, L tells to store the value as integer| x=10L|
|Complex| To represent complex values | x = 10+2i|
|Logical| To represent boolean values, true or false | x = TRUE|
|Character| To represent string values | x <- "One compiler"|
| raw | Holds raw bytes||

```java
var_logical <- FALSE  # Logical variable
cat("The data type of", var_logical," :",class(var_logical),"\n")  
  
var_numeric <- 797 # Numeric variable
cat("The data type of", var_numeric," : ",class(var_numeric),"\n")  
  
var_integer <- 53L  # integer variable
cat("The data type of", var_integer," : ",class(var_integer),"\n")  
  
var_complex <- 5+2i  # complex variable
cat("The data type of ", var_complex ,":",class(var_complex),"\n")  
  
var_char<- "One Compiler"  # character variable
cat("The data type of ", var_char ,":",class(var_char),"\n")  
  
var_raw <- charToRaw("Hello World")  # raw variable
cat("The data type of var_raw :",class(var_raw),"\n")
cat("var_raw is stored as", var_raw)
```
### Run [here](https://onecompiler.com/r/3vs6spse5)