String is a sequence of characters enclosed in double quotes. 

## How to declare strings

`String` keyword is used to declare strings and it is an alias for the `System.String` class. 

Below is the syntax:

```c#
Dim variableName As String
```

## How to initialize strings
```c#
Dim Name As String = "OneCompiler"
```


## String Functions

VB.net provides various in-built string functions which can manipulate strings. Some of the frequently used string functions are listed below:

| Function name | Description|
|----|----|
|str.Length| to return the length of string str|
|string.Concat(str1,str2)| to concatenate two strings str1 and str2|
|Copy(str1, str2)| To copy string str2 into string str1.|
|String.Compare(str1, str2)| returns 0 if str1 and str2 are the same and less than 0 if str1 < str2 and a positive number if str1 > str2|
|Str.Contains("value")| checks for the value in str|
|Str.Substring(start_Index, length)| Used to return a part of a specified string|
|Join(str, String())| concatenate all the elements of the given string array with the specified separator between each element.|
|Split(Char())| splits a string into substrings based on the characters in an array|
|str.ToUpper()| converts the string to upper case|
|str.ToLower()| converts the string to lower case|
|ToString()| to return instance of a string|
|Trim()| removes all leading and trailing whitespaces from a given string|
|Clone()| returns a reference to this instance of String|

## Examples

```vb
Public Module Program
	Public Sub Main(args() As string)
	      Dim str as String = "OneCompiler"
        Console.WriteLine("The length of the string is: " & str.Length) 'returns the length of the string
        
        Console.WriteLine(str.ToUpper()) 'converts to upper case
        Console.WriteLine(str.ToLower()) 'converts to lower case
        
        Dim str1 as String = "Hello "
        Dim str2 as String = "World!"
        Console.WriteLine(string.Concat(str1,str2)) 'string concatenation
        
        Dim str3 as String = string.Copy(str1) 'string copy
        Console.WriteLine(str3)
        
        Console.WriteLine(string.Compare(str1,str3)) 'string compare
        Console.WriteLine(string.Compare(str1,str2)) 'string compare
        
  	End Sub
End Module
```

### Check result [here](https://onecompiler.com/vb/3vu53ue6p)
