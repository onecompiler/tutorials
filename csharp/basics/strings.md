String is a sequence of characters enclosed in double quotes. 

## How to declare strings

Declaring strings is similar to one-dimensional character array. Below is the syntax:

```c#
string str;
```

## How to initialize strings
```c#
string str = "OneCompiler";
```


## String Functions

C# has various in-built string functions which can manipulate strings. Some of the frequently used string functions are listed below:

| Function name | Description|
|----|----|
|str.Length| to return the length of string str|
|+| to concatenate two strings|
|string.Concat(str1,str2)| to concatenate two strings str1 and str2|
|Copy(str1, str2)| To copy string str2 into string str1.|
|Compare(str1, str2)| returns 0 if str1 and str2 are the same and less than 0 if str1 < str2 and a positive number if str1 > str2|
|Join(str, String[])| concatenate all the elements of the given string array with the specified separator between each element.|
|Split(Char[])| splits a string into substrings based on the characters in an array|
|str.ToUpper()| converts the string to upper case|
|str.ToLower()| converts the string to lower case|
|ToString()| to return instance of a string|
|Trim()| removes all leading and trailing whitespaces from a given string|
|Clone()| returns a reference to this instance of String|

## Examples

```c#
using System;

namespace Strings
{
	public class Strings
	{
		public static void Main(string[] args)
		{
        string str = "OneCompiler";
        Console.WriteLine("The length of the string is: " + str.Length); //returns length of the string
        
        Console.WriteLine(str.ToUpper()); //converts to upper case
        Console.WriteLine(str.ToLower()); //converts to lower case
        
        String str1 = "Hello ";
        String str2 = "World!";
        Console.WriteLine(str1+str2); //string concatenation
        Console.WriteLine(string.Concat(str1,str2)); //string concatenation
        
        String str3 = string.Copy(str1); //string copy
        Console.WriteLine(str3); 
        
        Console.WriteLine(string.Compare(str1,str3)); //string compare
        Console.WriteLine(string.Compare(str1,str2)); //string compare
        
        String[] s = {"Happy", "fun", "learning"};
        Console.WriteLine(string.Join("**",s)); //string Join
        
        string str4 = "Welcome to One Compiler";  //string Split
        string[] str5 = str4.Split(' ');  
        foreach (string str6 in str5)  
        {  
            Console.WriteLine(str6);  
        }  
        
    }
	}
}
```

### Check result [here](https://onecompiler.com/csharp/3vtbhkheh)
