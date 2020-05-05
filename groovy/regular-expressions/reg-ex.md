Regular expressions are used to find a sub-string from a given string. By placing `~` infront of a string, you can define regular expressions. 

Usually single or double or triple quotations are used to declare strings, there is one more way which is more helpful is by defining the string using `/.../` . In this way, you don't need to escape a backslash character.

You can compare a string against a regular expression using `=~` and  you can access the result of the found by using indexing. 

## Example

```java
def found = "Hello world, happy learning" =~ /Hello/
if (found) {
  println "Found the word" 
  println "found contains $found"

  println found[0] // to get whole world

  // to get element by element
  println found[0][0]
  println found[0][1]
  println found[0][2]
  println found[0][3]
  println found[0][4]
}
```

### Check result [here](https://onecompiler.com/groovy/3vn2vdadj)

