Errors may occur due to a number of reasons. Like in most other programming languages, try-catch is used to handle the errors occured.

There are three types of errors in programming: 
## 1. Syntax Errors

Syntax errors are the ones which occur in compile time due to incorrect syntax.

### Example

```javascript
console.log(3+2; // missing )
```
## 2. Runtime Errors

Runtime errors are the ones which occurs during execution time. Though syntactically it is correct, it is calling an unknown function

### Example

```javascript
let x = 3;
let y = 10;

console.log(sum(x,y)); // calling unknown function
```

## 3. Logical Errors

Logical errors are the complex ones to find as they occur due to errors in your program logic. Based on the requirement you can put try-catch blocks to catch exceptions. 

## Syntax

```javascript
try {
    //code
} catch(err) {
    //code
}
```

## General errors on webpages

### chrome, Firefox, IE, Edge etc

* Press F12 to see the console which displays errors if any on the webpage.


