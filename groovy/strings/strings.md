Strings in Groovy are an ordered series of characters usually enclosed in quotations. single quotes (’), double quotes (“), or triple quotes (“””) can be used to enclose strings in Groovy. You can also use forward slash `/` or dollar-forward slash `$/` to enclose strings. If a string contains multiple lines, then triple quotes `“””`, or forward slash `/` or dollar-forward slash `$/` are used.

Similar to arrays, individual characters of a string can be accessed by it's indexing. Indices starts from `0` and hence `string[0]` represents it's first character present in the string. Negative indices indicates to count back from the end of the string.

## How to declare strings

```java
String string-var = value; 
```
value should be enclosed in either single quotes `’`, double quotes `“`, or triple quotes `“””` or forward slash `/` or dollar-forward slash `$/`.

## Example

```java
String str = "Happy learning!"; 
String str2 = 'Hello World';
String str3 = """Hey
Good 
Morning"""

println(str)
println(str2)
println(str3)

println(str[0]); // Prints first character of the string
println(str[-1]); //Print first character from the back 
println(str2[6..10]);//Prints series of characters starting from Index 6 to 10
println(str2[10..6]);//Prints series of characters starting from Index 10 to 6
```
### Check result [here](https://onecompiler.com/groovy/3vmvsd3kp)

## Basic String operations

|String Operation| Description|
|----|----|
| Concatenation|concatenation of strings can be done by using `+` operator.|
| Repetition|  repetition of strings can be done by using `*` operator.|
| String length | length() method is used to determine length of the string |

### Example

```java
// Different ways of declaring strings
String str = "Happy learning!"; 
String str2 = 'Hello World';
String str3 = """Hey
Good 
Morning"""
String str4 = /hello/
String str5 = /learning
is 
fun
!!/
String str6 = $/learning
is 
more
exciting..
/$

println(str)
println(str2)
println(str3)
println(str4)
println(str5)
println(str6)

println("first character of string is $str[0]"); // Prints first character of the string
println(str[-1]); //Print first character from the back 
println(str2[6..10]);//Prints series of characters starting from Index 6 to 10
println(str2[10..6]);//Prints series of characters starting from Index 10 to 6

```
### Check result [here](https://onecompiler.com/groovy/3vmvwpgq7)

## String Methods

|String Method| Description|
|----|----|
| indexOf() |Returns the index of first occurrence of the specified substring or character in the given string.|
| subString() | Returns a new String which is a substring of the given String.|
| toUpperCase() | Converts all of the characters in the given String to upper case.|
| toLowerCase() | Converts all of the characters in the given String to lower case.|
| matches() | Checks whether the given String matches the specified regular expression.|
| padLeft() | Pad the given String with white spaces padded to the left.|
| padRight()| Pad the given String with white spaces padded to the right.|
| center()| to return a new String of length numberOfChars consisting of the recipient padded on the left and right with space characters.
| compareToIgnoreCase() | Compares two strings lexicographically by ignoring case differences.|
| concat() | Concatenates the given String to the end of another String.
| eachMatch() | Processes each regex group to perform an action for each matched substring of the given String.|
| endsWith() | to check whether the given string ends with a specified suffix.|
| equalsIgnoreCase() | Compares the given String with another String by ignoring case differences.|
| getAt() | Returns string value at the given index position|
| minus() | Removes the specified value part of the given String.|
| next() | It increments the last character in the given String. This method is called by the ++ operator.|
| plus() | Appends a given String|
| previous() | Usually this method is called by the -- operator for the CharSequence.|
| replaceAll() | Replaces all occurrences of a substring with another given string|
| reverse() | Creates a new String which is the reverse of the given String.|
| split() | Splits the given String around matches of the given regular expression.|
