Structure is a user-defined data type where it allows you to combine data of different data types. In a way, Structures are similar to arrays but the difference is in type of the data. Array is a collection of similar data but structures combine different types of data.

## How to define a structure

`struct` keyword is used to define a structure. 

```c
type structure_name struct{

   member definition;
   member definition;
   ...
   member definition;
} 
```

## How to declare structure variables

Structure variables can be declared as below:

```c
var structure-variable structure-name 
```

## Example 

```c
// structure definition
type mobile struct {
    model string
    brand string
    cost int 
}

// Declaring structure variables 
var m1, m2 mobile
```

## How to access structure members

You can access the structure member using `variable_name.membername`


## Example 
```c
package main
import "fmt"
// structure definition
type mobile struct {
    model string
    brand string
    cost int 
}

func main() {
  // Declaring structure variables 
  var m1 mobile

  // Accessing Structure members
  m1.model = "11 Pro"
  m1.brand = "iPhone"
  m1.cost = 999

  fmt.Println( "Model name: ", m1.model)
  fmt.Println( "Brand name: ", m1.brand)
  fmt.Println( "Cost: ", m1.cost)
}
```

### Check result [here](https://onecompiler.com/go/3vq56vnw4)
