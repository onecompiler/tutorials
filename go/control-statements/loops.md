# For

For loop is used to iterate a set of statements based on a condition. This is the only loop in Go language. It has two variants:

1. Counter-controlled iteration and
2. Condition-controlled iteration.

Objects which are created inside loop gets destroyed once the loop is completed.

## 1. Counter-controlled For loop

### Syntax

```c
for Initialization; Condition; Increment/decrement {  
//code  
} 
```
### Example

```c
package main
import "fmt"

func main() {
   for x := 1; x < 6; x++ {  
      fmt.Println(x)  
   }  
}
```

#### Check Result [here](https://onecompiler.com/go/3vpqmww3e)

## 2. Conditional controlled For loop

This loop looks similar to while loop in other programming languages.

### Syntax

```c
for condition {  
//code  
} 
```
### Example

```c
package main
import "fmt"

func main() {
  x := 1;
  for x <= 5 {
  fmt.Println(x)  
  x += 1 
  }
}
```
### Check result [here](https://onecompiler.com/go/3vpqn6j2c)
