Array is a collection of similar data which is stored in continuous memory addresses. Array values can be fetched using indices. Index starts from 0 to size-1.

## How to declare an array?

```c
var array-name[size] data-type;
```
## How to assign values to an array?

```c
array-name := [size] data-type {value0,value1,…,value_size-1} 
```

## Examples

```c
var fruits [3] string //Declaring a string array of size 3  
fruits[0] = "Mango"
fruits[1] = "Apple"
fruits[2] = "Orange"
arr := [...] int {1,2,3,4,5} //Declaring a integer array of size 5
```

# How to access array elements

Array elements can be accessed by using indices. Array indices starts from `0`.  `Array[n-1]` can be used to access nth element of an array.

## Examples

```c
package main
import "fmt"

func main() {
  
var fruits [3] string;//Declaring a string array of size 3  
fruits[0] = "Mango"
fruits[1] = "Apple"
fruits[2] = "Orange"
fmt.Println(fruits);
fmt.Println("Size of fruits array is", len(fruits));

num := [5] int {1,2,3,4,5} //Declaring a integer array of size 5
fmt.Println(num);
fmt.Println("Size of num array is", len(num));

fmt.Println(num[0]); // accessing array elements
}
```
### Check Result [here](https://onecompiler.com/go/3vpx42ezt)
