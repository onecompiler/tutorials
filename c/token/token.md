
Token can be referred as the smallest possible unit in C. Token is divided into six categories as follows.

## 1. Keywords 

Keywords are the reserved words in a programming language. C supports 31 keywords as below

```c
auto         double      int        struct
break        else        long       switch
case         enum        register   typedef
char         extern      return     union
const        float       short      unsigned
continue     for         signed     void
default      goto        sizeof     volatile
do           if          static     while
```
## 2. Identifiers

Identifiers are the user defined names for variables, functions and arrays.

Rules for defining an identifier:
* They must be less than or equal to 31 characters.
* No special characters.
* Must start with a letter or under score.
* Can contain letters, digits, or underscore only.

## 3. Strings

Strings are an array of characters ended with null character. Characters are enclosed in single quotes where as strings are always enclosed in double quotes.

```c
char str[]="onecompiler";
```

## 4. Operators

Operators are the symbols which specifies an action when applied on variables.

* Arithmetic operators
* Relational Operators
* Logical Operators
* Assignment Operators
* Conditional Operators
* Bitwise Operators

## 5. Constants

Constants are the fixed values. Constant values can't be changed once defined.

``` c
const datatype variable_name;
```

## 6. Special Characters

Few characters have special meaning and hence these can't be used for other purposes.

* `{}` : spcifies start and end of code blocks
* `[]` : Used for arrays
* `()` : Used for functions
* `,` : used to seperate variables, constants etc
* `*` : used for pointers
* `#` : used as a macro processor.

