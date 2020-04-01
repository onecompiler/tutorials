String is an ordered series of characters usually enclosed in single quotes (') or double quotes (“) quotations.

## How to declare strings

```java
$string_var = value; 
```
value should be enclosed in either single quotes `'` or double quotes `“`.

## Example

```perl

$str1 = 'Hello World! ';
$str2 = "Happy learning!!"; 

print($str1);
print($str2);
```
### Check result [here](https://onecompiler.com/perl/3vnmqz42z)

# String Functions

Below are some of the useful string functions in Perl.

| String Function |	Description|
|----|----|
|length| This function is used to return the number of characters of a given string|
|substr| This method is used to modify a substring in a string|
|index|	Searches for a substring in the given string and returns the position of the first occurrence of the substring if found|
|rindex| Similar to index but searches for a substring from right to left|
|reverse| This function is used to reverse a string|
|lc| This function is used to convert the specified string to lowercase|
|uc| This function is used to convert the specified string to uppercase|
|crypt|	This function is used to encrypt password|
|q/string/|	used to create single-quoted strings|
|qq/string/| used to create double-quoted strings|
|chr| to return ASCII or UNICODE character of a number|
|hex| used to convert a hexadecimal string to it's equivalent decimal value|
|oct| used to convert an octal number to it's equivalent decimal value|
|ord| returns the ASCII value of the first character of a string|
|sprintf| Formats string  provided by the user and returns the formatted string to be used with print()|

## Example

```perl
#!/usr/bin/perl
use warnings;
use strict;

# Find the length of a string
my $str = "Hello world! Happy learning!!";
print(length($str),"\n"); 

# extract substring from a string
my $substr1  = substr($str, 0, 5);   
my $substr2    = substr($str, -10);     
 
print($substr1,"\n");
print($substr2,"\n");
 
my $i = "Happy";
my $p = index($str,$i); 
print(qq\Index of substring "$i" is "$p" in the string: "$str"\,"\n");

print("Upper case of the string:\n");
print(uc($str),"\n");
 
print("Lower case of the string:\n");
print(lc($str),"\n");
```
### Check result [here](https://onecompiler.com/perl/3vnqhk3bu)
