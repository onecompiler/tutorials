
# Strings in Java

* String is a sequence of characters.
* Strings are used to store text data.
* Strings are declared using double quotes. 
* Strings are objects in Java.
* `java.lang.String` class is used to create a string object
* String objects are immutable means once defined can't be changed.

### Example

```java
String name = "OneCompiler";
```
## String Methods

String class offers  various useful string methods which are very helpful while dealing with string operations. Let us learn some of the useful string methods as below.

Consider str = "onecompiler" for understanding the below methods. 

| Method| Description| Example|
|----|----|----|
|char charAt(int index)|returns char value at the specific index| str.charAt(0) //prints o| 
|int compareTo(String str)| to compare two strings lexicographically | Str1.compareTo(Str2) //returns 0 if str1 and str2 are equal|
|int length()|returns string length| str.length()| 
|static String format(String format, Object... args)|returns a formatted string.| String.format("String is %s",str); |
|String substring(int beginIndex, int endIndex)| to return substring from given begin index to end index.| str.substring(0,3) //prints one| 
|String substring(int beginIndex)| to return substring from given begin index| str.substring(3) //prints compiler|
|boolean contains(CharSequence s)|returns true or false after matching the sequence given in the string| str.contains("compiler") // returns true|
|static String join(CharSequence delimiter, CharSequence... elements)|returns a joined string.| String.join("..","Hello","Happy", "Learning"); //returns Hello..Happy..Learning|
|boolean equals(Object another)|checks the equality of string with another and returns true if they are equal.| str1.equals(str2);
|boolean isEmpty()|to check if the given string is empty.|  str.isEmpty() // returns false|
|String concat(String str)|concatenates the provided string with the another string.| str.concat(" is used to compile code online")|
|String replace(char old, char new)|replaces all occurrences of the specified char value with new char value.|str.replace('r','t');|
|String replace(CharSequence old, CharSequence new)|replaces all occurrences of the specific CharSequence with new one.|str.replace('one','Online');|
|static String equalsIgnoreCase(String another)|compares another string with out considering case.| str1.equalsIgnoreCase(str2)|
|String[] split(String regex, int limit)|returns a split string matching regex and limit. here limit is optional| str.split("\\s")//splits string based on whitespaces|
|int indexOf(String substring, int fromIndex)|returns the specified substring index starting with given index. here index is optional| str.indexOf("compiler",2);|
|String toLowerCase()|returns a string in lowercase.|str.toLowercase();|
|String toUpperCase()|returns a string in uppercase.|str.toUpperCase();|
|String trim()|removes beginning and trailing spaces of a given string.|str.trim();|

# Exercises

## 1. How do you extract all integers from the given string?

## 2. How do you determine length or size of a String?

## 3. How to compare two strings?

## 4. How to reverse a string?

## 5. How to remove leading zero's from a string?

## 6. How to count number of lines, words, letters in a text file?

## 7. How to check if a string contains only alphabets?

## 8. Which is the preferred way to store passwords? 
