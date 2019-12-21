Strings can be enclosed with in either single quotes, or double quotes.

## Multi-line strings

Multi-line strings should be enclosed within three double quotes or three single quotes.

```py
str = """Line1
line2
line3..."""
```
[or]

```py
str = '''Line1
line2
line3...'''
```
## Slicing

Below syntax shows how to extract a sub-string.
```py
str[str_index:end_index]
```

## Negative indexing

Negative indexing is used to extract the sub-string from the end.

```py
str[-str_index:-end_index]
```
## String functions

|Method name|Syntax|Description|
|------|-----|-----|
|strip()|str.strip()|to remove spaces in the given str|
|lstrip()|str.lstrip(characters)| to trim the left side of the given characters in the given string|
|rstrip()|str.rstrip(characters)| to trim the right side of the given characters in the given string|
|lower() or casefold()|str.lower()|to convert string to lowercase, casefold() also can be used to convert the string to lowercase.|
|upper()|str.upper()| to convert the string to upper case|
|replace()|str.replace("str to be replaced","new string to replace")|to replace the specific portion of a string with a new string given to replace|
|split()|str.split("seperator")|to split the string based on the seperator specified|
|len()|len(str)|to return the length of str|
|`in` or `not in`|substring `in` str| to check if the substring is present in the given str|
|+| str1 + str2| to concatenate string 1 and string 2|
|format()|str.format(num)|to combine string and a number|
|capitalize() or title|str.capitalize()|	to Convert first character to upper case, title() also can be used|
|center()|string.center(length, character)|	Returns a centered string where length is the length of the returned string and character is optional which specifies the padded character |
|count()|str.count(substr)|	to return number of times a specified sub-string occurs in a string|
|encode()|str.encode(encoding=encoding, errors=errors)|	to return an encoded version of the string, both the parameters are optional. default encoding is UTF-8, and errors can be backslashreplace, ignore, namereplace, strict, replace, xmlcharrefreplace|
|find()|str.find(substr)|	Searches for the sub-stringin the given string and returns the first position of the sub-str in the string given if found. if the sub-string is not found then returns -1|
|index()|str.index(substr, start, end)|	Searches the string for a specified sub-string and returns the position of the first occurence of where the sub-string is found, start specifies the starting position to search and end specifies the end position|
|join()|str.join(array)|	Joins all the elements present in the array or a tuple to the end of the string
|ljust()|str.ljust(length, character)| to return a left justified version of the string, where length of the required string and character is optional which specifies the padded value, by default it is space.|
|rjust()|str.ljust(length, character)|	to return a right justified version of the string, where length of the required string and character is optional which specifies the padded value, by default it is space.|
|partition()|str.partition(substr)| to returns an array where the given string is partitioned into three parts based on the substr specidied|
|swapcase()|str.swapcase()|	Swaps lower case to upper case and uppercase to lowercase in the given str|
|zfill()|str.zfill(len)| pads the given string with a specified number of zeros at the beginning|

## String boolean methods

|Method name|Syntax|Description|
|------|-----|-----|
|isalnum()|str.isalnum()|	Returns True only if all characters in the given string are alphanumeric|
|isalpha()|str.isalpha|	Returns True if all characters in the given string are alphabets|
|isdecimal()| str.isdecimal()|	Returns True if all characters in the given string are decimals|
|isdigit()|str.isdigit()|	Returns True if all characters in the given string are digits|
|isidentifier()| str.isidentifier()|	Returns True if the given string is an identifier|
|islower()|str.islower()|	Returns True if all characters in the given string are in lower case|
|isnumeric()|str.isnumeric()|	Returns True if all characters in the given string are numeric|
|isprintable()|str.isprintable()|	Returns True if all characters in the given string are printable|
|isspace()|str.isspace()|	Returns True if all characters in the given string are whitespaces|
|istitle()|str.istitle()|	Returns True if the given string follows the rules of a title|
|isupper()|str.isupper()|	Returns True if all characters in the given string are upper case|
|endswith()|str.endswith(value, start, end)|	Returns true if the string ends with the  value given and start indicates the starting position of the search and end indicates the end position|
|startswith()|str.startswith(value, start, end)|	Returns true if the string starts with the  value given and start indicates the starting position of the search and end indicates the end position|
