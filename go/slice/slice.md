As we all know that array has a fixed size and hence you can't be able to resize based on your requirements. On the other hand, Slice is dynamically-sized and provide flexible view into the elements of an array. Hence, Slices are more common than arrays in Go.

## Syntax

A slice can be formed from an array using a low and high bound indices which are separated by a colon:

```c
array-name[lowIndex : highIndex]
```

## Example

```c
package main
import "fmt"

func main() {
	num := [5]int{1, 2, 3, 4, 5}

	var slc []int = num[1:4]
	fmt.Println(slc)
}
```
### Check result [here](https://onecompiler.com/go/3vq7njk4f)


Some interesting facts about Slice in Go

* Slices never store any data
* Slices just describes the section of an array
* If you modify the elements present in a Slice, it's corresponding array elements also will see changes.
* If you omit high or low bounds while slicing, Go automatically use their defaults instead. The default values for low bound is zero and for the high bound is the length of the slice.
* Slice can be created by using built-in function `make`. The below creates a slice `[0 0 0 0 0]`

    ```c
    arr := make([]int, 5)
    ```
* You can append new elements to Slice by using a built-in `append` function. 
    ```c
    package main
    import "fmt"

    func main() {
        arr := make([]int, 1)
        fmt.Println(arr)
        
        arr[0] = 1 // Changing Slice value
        fmt.Println(arr)
        
        arr = append(arr, 2, 3, 4, 5) // Appending elements to Slice
        fmt.Println(arr)
    }
    ```

### Check Result [here](https://onecompiler.com/go/3vq7q5aap)

