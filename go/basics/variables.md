Variables are like containers which holds the data values. A variable specifies the name of the memory location. 

`var` statement is used to declare a list of variables with data type at last. You can declare variables either at package level or at function level.

## How to declare variables

```c
var variable-name data-type
```
Mentioning datatype is optional.

## How to assign variables

`:=` is used as short assignment and you no need to declare `var` as well. Please note you can use short assignment (`:=`) only inside functions. `:=` is not available outside functions. 

```c
variable-name := value
```

## Naming convention of variables

* Variable names generally follow Camel case style. For example, firstName. Please note second word first letter(N in Name) is capital.
* Variable name should follow either general English expression or shorthand. For example, employeeID can be given as `eid`.
* It's good to start with bool variable names with `Has`, `Is`, `Can` or `Allow`, etc.
* It is always advisable to give some meaningful names to variables.

### Example

```c

package main

import ("fmt")

var str string = "Hello World" // declaring string variable at package level
var boolean   bool = true  // declaring bool variable at package level
var integer uint32     =  7979  // declaring uint32 variable at package level

func main() {
  var i int  // declaring int variable at functional level
  j := 99 // short assignment without var declaration
  fmt.Printf("Value: %v is of Type: %T\n", integer, integer)
  fmt.Printf("Value: %v is of Type: %T\n", i, i)
  fmt.Printf("Value: %v is of Type: %T\n", j, j)
  fmt.Printf("Value: %v is of Type: %T\n", str, str)
  fmt.Printf("Value: %v is of Type: %T\n", boolean, boolean)
}
```

### Check result [here](https://onecompiler.com/go/3vpkzg55z)