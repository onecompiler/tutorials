Strings can be enclosed with in either single quotes, or double quotes. R stores strings in double quotes even if you declare them with single quotes.

```java
str1 <- "Hello World!"
print(str1)
str2 <- 'Happy learning!!'
print(str2)
```
# String functions

## 1. paste()

Strings are concatenated using paste() function.

```java
paste(..., sep = " ", collapse = NULL)
```

* **...** : represents any no of arguments
* **sep** : indicates the seperator and it is optional
* **collapse** : eliminates space in between two strings and it is optional.

### Example
```java
str1 <- "Hello World!"
str2 <- 'Happy learning!!'

print(paste(str1, str2))

print(paste(str1, str2, sep = "**"))

print(paste(str1, str2, sep = "           ", collapse = NULL))
```
### Check Result [here](https://onecompiler.com/r/3vs9wgsyv)

## 2. format()

format() function is used to format Numbers and strings  to a specific style.

```java
format(vector, digits, nsmall, scientific, width, justify = c("left", "right", "centre", "none")) 
```

* **vector** : vector input.
* **digits** : total number of digits displayed.
* **nsmall** : minimum number of digits to be displayed at the right side of the decimal point.
* **scientific**: scientific notation will be displayed if it is set to TRUE.
* **width** : minimum width to be displayed by padding blanks in the beginning.
* **justify** : specifies the position of the string whether it's left, right or center.

### Example

```java
x <- format(53.2, width = 7)
print(x)

str <- format("Good morning", width = 20, justify = "c")
print(str)
```
### Check Result [here](https://onecompiler.com/r/3vs9xavys)

## 3. nchar()

This function is used to count the number of characters including spaces in the given string.

```java
nchar(x)
```
### Example

```java
str <-"Hello World!"
print(nchar(str))
```
### Check result [here](https://onecompiler.com/r/3vs9xhvf4)

## 4. substring()

This function is used to extract part of a string.

```java
substring(vector,begin,end)
```
* **vector** : vector input.
* **begin** : beginning index
* **end** : ending index

### Example
```java
str <-"Hello World!"
print(substring(str, 3, 5))
```
### Check Result [here](https://onecompiler.com/r/3vs9y5qa2)

## 5. toupper() & tolower() 

These functions are used to change the case of a string
```java
toupper(vector-input)
tolower(vector-input)
```
### Example
```java
str <-"Hello World!"
cat("In upper case: ", toupper(str), "\n")
cat("In lower case: ", tolower(str), "\n")
```
### Check Result [here](https://onecompiler.com/r/3vs9y8bwx)