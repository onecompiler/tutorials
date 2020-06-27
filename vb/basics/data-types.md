As the name suggests, data-type specifies the type of the data present in the variable. Variables must be declared with a data-type. 

| Data type | Description | Range | Memory Size|
|----|----|----|----|----|
|byte|	used to store unsigned integer|	0 to 255| 1 byte|
|sbyte|	used to store signed integer|	-128 to 127| 1 byte|
|ushort| used to store unsigned integers|	0 to 65,535 | 2 bytes|
| integer| used to store signed integers|-2,147,483,648 to 2,147,483,647|4 bytes| 
|long | used to store signed integers|-9,223,372,036,854,775,808 to 9,223,372,036,854,775,807| 8 bytes|
|double| used to store fractional numbers|15 decimal digits| 8 bytes|
|char|used to store a single character enclosed in single quote|one character|2 bytes|
|boolean| Boolean data type|Stores either true or false | 1 bit|
|date| Used to store date values|0:00:00 January 1, 0001 to 23:59:59 December 31, 9999|8 bytes|
|decimal|used to store decimal values| 0 to +/-7.9228162514264337593543950335 with 28 places to the right of the decimal|16 bytes|
|String| Stores a sequence of characters enclosed in double quotes| Sequence of Characters|2 bytes per character|
|object | Object can be represented as base type of all other types.||4 bytes on 32-bit platform and 8 bytes on 64-bit platform|

## Examples

```c#
   Dim byteVar As Byte
   Dim intVar As Integer
   Dim doubleVar As Double
   Dim dateVar As Date
   Dim charVar As Char
   Dim strVar As String
   Dim boolVar As Boolean
```

## Type Conversions

|Function|Description|
|----|----|
|CChar(exp)| to convert the expression to a Char data type.|
|CStr(exp)| to convert the expression to a String data type.|
|CBool (exp)| to convert the expression to a Boolean data type.|
|CDate(exp)| to convert the expression to a Date data type.|
|CInt(exp)| to convert the expression to an Integer data type.|
|CLng(exp)| to convert the expression to a Long data type.|
|CDec(exp)| to convert the expression to a Decimal data type.|
|CDbl(exp)| to convert the expression to a Double data type.|
|CByte (exp)| to convert the expression to a byte data type.|
|CSByte(exp)| to convert the expression to a Byte data type.|
|CShort(exp)| to convert the expression to a Short data type.|
|CObj(exp)| to convert the expression to an Object data type.|

