Pointer is a variable which holds the memory information(address) of another variable.

* Pointers helps in dynamic memory management.
* Improves execution time.
* Pointers pass reference to a variable instead of passing a copy of the variable which helps in reduction of memory usage.

## Syntax

```c
var pointername *datatype;
```

## Example : 1

```c
package main
import "fmt"

func main() {
    num := 10;     
    var ptr *int;   // pointer variable

    ptr = &num;

   fmt.Println("Value of the variable: ", *ptr);

   fmt.Println("Value at the address: ", ptr);

}
```
### Check result [here](https://onecompiler.com/go/3vq282824)


## Example : 2

```c
package main
import "fmt"

func main() {

x := 10;     
var ptr *int;   // pointer variable

/* ptr = x; // Error because ptr is address and x is value

*ptr = &x;  // Error because &x is address and *ptr is value  */

ptr = &x; // valid because &x and ptr are addresses

*ptr = x; // valid because both x and *ptr values 

fmt.Println("Value of *ptr: ", *ptr);
fmt.Println("Value of &x: ", &x);
fmt.Println("Value of ptr: ", ptr);
fmt.Println("Value of x: ", x);
}
```
### Check result [here](https://onecompiler.com/go/3vq288zqr)

