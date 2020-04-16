String is a read-only slice of bytes. Strings are immutable in Go means once you created you can't modify the contents of a string. When you define a string with characters, you are actually storing it's bytes not the characters.

# How to declare strings

## Syntax

```c
var string-name = value;
```
For example, you can define a string variable like below:
```c
var str = "OneCompiler";
```
## Example

```c
package main  
import ("fmt"  
      "reflect"  
)  
func main()  {  
   var str = "One Compiler"  
   fmt.Println(str)  
   fmt.Println(reflect.TypeOf(str))  
   
   fmt.Printf("Quoted string is: ")
   for i := 0; i < len(str); i++ {
    fmt.Printf("%+q", str[i])
   }
  
   fmt.Printf("\n")
   fmt.Printf("Hex bytes of the string is: ")
   for i := 0; i < len(str); i++ {
      fmt.Printf("%x ", str[i])
   }
   fmt.Printf("\n")
} 
```
### Check result [here](https://onecompiler.com/go/3vq3x3vjx)

# String Functions

Go has various in-built string functions which can manipulate strings. Below are some of the frequently used string functions.

| Function name | Description|
|----|----|
|len(str)| to return the length of string str|
|strings.Compare(a, b) | Compares two strings a and b lexically and returns `0` if the both strings are equal and `1` if string 1 is greater than string 2 and `-1` if string 1 is less than string 2.|
|strings.Contains(str, substr)| returns true if substring is found in the string str|
|strings.ToUpper(str)| to change the str to Upper Case|
|strings.ToLower(str)| to change the str to lower Case|
|strings.HasPrefix(str,"prefix")| returns true if the string str is starting with a prefix |
|strings.HasSuffix(str,"suffix")|  returns true if the string str is ending with a suffix |
|strings.Index(str, substr)| searches for a particular text within a string and returns it's index. If not found then it will returns -1.|
|strings.Join(stringSlice []string, sep string)| contanates the elements of an slice with seperators|
|strings.Count(str, sep string)| returns the count of number of non-overlapping instances of a character/string/text in string|

## Examples

```c
package main
import "fmt"
import "strings"

func main() {
   var str =  "Good morning!"
   
  // String Length
   fmt.Println("Length of the str is:", len(str))  
   
  // string compare
  fmt.Println("\nComparing Strings:")
  fmt.Println(strings.Compare("Apple", "Banana"))
  fmt.Println(strings.Compare("Banana", "Apple"))
  fmt.Println(strings.Compare("Apple", "Apple"))
  
  // string contains
  fmt.Println("\nWhether String contains?")
  fmt.Println(strings.Contains("Apple", "A")) 
  fmt.Println(strings.Contains("Apple", "Ba")) 
  
  //Uppercase and Lowercase
  fmt.Println("\nChanging Case")
  fmt.Println(strings.ToUpper("Hello world!"))
  fmt.Println(strings.ToLower("happy learning!!"))
 
  // string prefix and suffix checking
  fmt.Println("\nChecking Prefix and suffix:")
  fmt.Println(strings.HasPrefix("Hello World", "Hello"))
  fmt.Println(strings.HasPrefix("Hello World", "hello"))
  fmt.Println(strings.HasSuffix("Hello World", "World"))
  
  //String Index
  fmt.Println("\nFinding Index:")
  fmt.Println(strings.Index("Hello World", "World"))
  
  //String Join
  fmt.Println("\nUsing String Join:")
  sampleSlice := []string{"one", "two", "three", "four", "five"}
  fmt.Println(strings.Join(sampleSlice,"*"))
  
  // String count
  fmt.Println("\nCount:")
  fmt.Println(strings.Count("Hello World", "World"))
  fmt.Println(strings.Count("Hello World", "l"))
  
}
```

### Check result [here](https://onecompiler.com/go/3vq4z24gu)

