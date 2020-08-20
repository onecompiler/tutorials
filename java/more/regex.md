* Regex means a Regular Expression
* It is a search pattern which is formed by a sequence of characters
* Used to search patterns in a string
* To use Regex class we need to import `java.util.regex`

## Methods in `java.util.regex`
### 1. Pattern
* Defines a pattern that is to be searched 
* A pattern is created using the function `compile()`
* It has 2 parameters, First parameter used to indicate the pattern that is searched and the other parameter is a flag to tell the case-sensitivity

### Patterns
* \[xyz\] : Finds a character from options between the brackets
* \[^xyz\] : Finds a character from options not between the brackets

### Flags
* pattern.LITERAL : special characters also treated as normal characters
* pattern.CASE_INSENSITIVE : case of letters is not considered

### 2. Matcher
Used to search the patterns

### 3. PatternSyntaxException
Used to tell syntax error in a regex pattern

### Example


```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class Main {
  public static void main(String[] args) {
    Pattern pattern = Pattern.compile("onecompiler", Pattern.CASE_INSENSITIVE);
    Matcher matcher = pattern.matcher("Welcome to onecompiler");
    boolean search = matcher.find();
    if(search) {
      System.out.println("found");
    } else {
      System.out.println("Not found");
    }
  }
}
```
### check output [here](https://onecompiler.com/java/3w4fk3y7w)