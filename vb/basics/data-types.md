# Data Types in VB.NET

VB.NET is a strongly-typed language, meaning every variable must have a declared type. The .NET Framework provides a rich set of data types to handle various kinds of data.

## Value Types vs Reference Types

### Value Types
- Stored directly in memory
- Include all numeric types, Boolean, Char, Date, and structures
- Copied by value

### Reference Types
- Store references to memory locations
- Include String, arrays, classes, and interfaces
- Copied by reference

## Numeric Data Types

| Data Type | .NET Type | Range | Size | Example |
|-----------|-----------|--------|------|---------|
| Byte | System.Byte | 0 to 255 | 1 byte | `Dim age As Byte = 25` |
| SByte | System.SByte | -128 to 127 | 1 byte | `Dim temp As SByte = -10` |
| Short | System.Int16 | -32,768 to 32,767 | 2 bytes | `Dim count As Short = 1000` |
| UShort | System.UInt16 | 0 to 65,535 | 2 bytes | `Dim port As UShort = 8080` |
| Integer | System.Int32 | -2.1 billion to 2.1 billion | 4 bytes | `Dim population As Integer = 1000000` |
| UInteger | System.UInt32 | 0 to 4.2 billion | 4 bytes | `Dim size As UInteger = 3000000` |
| Long | System.Int64 | -9.2 quintillion to 9.2 quintillion | 8 bytes | `Dim bigNumber As Long = 9999999999` |
| ULong | System.UInt64 | 0 to 18.4 quintillion | 8 bytes | `Dim huge As ULong = 18000000000000000000` |

### Floating-Point Types

| Data Type | .NET Type | Precision | Range | Size | Example |
|-----------|-----------|-----------|--------|------|---------|
| Single | System.Single | 7 digits | ±1.5 × 10⁻⁴⁵ to ±3.4 × 10³⁸ | 4 bytes | `Dim price As Single = 19.99F` |
| Double | System.Double | 15-16 digits | ±5.0 × 10⁻³²⁴ to ±1.7 × 10³⁰⁸ | 8 bytes | `Dim pi As Double = 3.14159265359` |
| Decimal | System.Decimal | 28-29 digits | ±1.0 × 10⁻²⁸ to ±7.9 × 10²⁸ | 16 bytes | `Dim money As Decimal = 19.95D` |

### Using Numeric Types

```vb
' Integer literals
Dim dec As Integer = 42         ' Decimal
Dim hex As Integer = &H2A       ' Hexadecimal (42)
Dim oct As Integer = &O52       ' Octal (42)
Dim bin As Integer = &B101010   ' Binary (42)

' Type suffixes
Dim intVal As Integer = 100I    ' Integer
Dim longVal As Long = 100L      ' Long
Dim singleVal As Single = 3.14F ' Single
Dim doubleVal As Double = 3.14R ' Double
Dim decimalVal As Decimal = 19.95D ' Decimal

' Numeric operations
Dim sum As Integer = 10 + 5
Dim product As Double = 3.14 * 2.0
Dim quotient As Double = 10.0 / 3.0
Dim remainder As Integer = 10 Mod 3
Dim power As Double = Math.Pow(2, 8)
```

## Text Data Types

### Char

```vb
' Single Unicode character
Dim letter As Char = "A"c       ' Note the 'c' suffix
Dim digit As Char = "9"c
Dim symbol As Char = "$"c
Dim unicode As Char = ChrW(65)  ' 'A' from Unicode value

' Char methods
Dim isLetter As Boolean = Char.IsLetter(letter)
Dim isDigit As Boolean = Char.IsDigit(digit)
Dim upper As Char = Char.ToUpper("a"c)
```

### String

```vb
' String creation
Dim name As String = "VB.NET"
Dim empty As String = String.Empty
Dim nullStr As String = Nothing

' String operations
Dim firstName As String = "John"
Dim lastName As String = "Doe"
Dim fullName As String = firstName & " " & lastName  ' Concatenation
Dim interpolated As String = $"{firstName} {lastName}"  ' Interpolation

' String methods
Dim length As Integer = name.Length
Dim upper As String = name.ToUpper()
Dim lower As String = name.ToLower()
Dim trimmed As String = "  spaces  ".Trim()
Dim substring As String = name.Substring(0, 2)  ' "VB"
Dim replaced As String = name.Replace(".", " ")  ' "VB NET"

' String comparison
Dim areEqual As Boolean = String.Equals(name, "VB.NET")
Dim compareResult As Integer = String.Compare(name, "vb.net", True)  ' Case-insensitive
```

## Boolean Type

```vb
Dim isActive As Boolean = True
Dim isComplete As Boolean = False

' Boolean operations
Dim andResult As Boolean = True And False     ' False
Dim orResult As Boolean = True Or False       ' True
Dim xorResult As Boolean = True Xor True      ' False
Dim notResult As Boolean = Not True           ' False

' Short-circuit evaluation
Dim andalsoResult As Boolean = True AndAlso False  ' False (short-circuit)
Dim orelseResult As Boolean = True OrElse False    ' True (short-circuit)
```

## Date and Time

```vb
' Date creation
Dim today As Date = Date.Today
Dim now As DateTime = DateTime.Now
Dim specificDate As Date = New Date(2024, 12, 25)
Dim specificTime As DateTime = New DateTime(2024, 12, 25, 10, 30, 0)

' Date operations
Dim tomorrow As Date = today.AddDays(1)
Dim nextMonth As Date = today.AddMonths(1)
Dim dayOfWeek As DayOfWeek = today.DayOfWeek
Dim dayOfYear As Integer = today.DayOfYear

' Date formatting
Dim shortDate As String = today.ToShortDateString()  ' "12/25/2024"
Dim longDate As String = today.ToLongDateString()    ' "Monday, December 25, 2024"
Dim customFormat As String = now.ToString("yyyy-MM-dd HH:mm:ss")

' TimeSpan
Dim startTime As DateTime = DateTime.Now
' ... some operation ...
Dim endTime As DateTime = DateTime.Now
Dim duration As TimeSpan = endTime - startTime
Console.WriteLine($"Operation took {duration.TotalMilliseconds} ms")
```

## Object Type

```vb
' Object can hold any type
Dim anything As Object
anything = 42           ' Integer
anything = "Hello"      ' String
anything = True         ' Boolean
anything = New Date()   ' Date

' Type checking
If TypeOf anything Is String Then
    Dim str As String = CType(anything, String)
    Console.WriteLine($"String value: {str}")
End If

' GetType method
Dim type As Type = anything.GetType()
Console.WriteLine($"Type: {type.Name}")
```

## Arrays

```vb
' Single-dimensional arrays
Dim numbers(4) As Integer  ' Array of 5 integers (0-4)
Dim names() As String = {"Alice", "Bob", "Charlie"}
Dim scores As Integer() = New Integer() {90, 85, 92, 88}

' Multi-dimensional arrays
Dim matrix(2, 2) As Integer  ' 3x3 matrix
Dim cube(,,) As Double = New Double(2, 2, 2) {}  ' 3x3x3 cube

' Jagged arrays
Dim jagged()() As Integer = New Integer(2)() {}
jagged(0) = New Integer() {1, 2}
jagged(1) = New Integer() {3, 4, 5}
jagged(2) = New Integer() {6}
```

## Nullable Types

```vb
' Nullable value types
Dim nullableInt As Integer? = Nothing
Dim nullableDate As Date? = Nothing

' Checking for null
If nullableInt.HasValue Then
    Console.WriteLine($"Value: {nullableInt.Value}")
Else
    Console.WriteLine("No value")
End If

' Null coalescing
Dim value As Integer = If(nullableInt, 0)  ' Use 0 if null
```

## Enumerations

```vb
' Define enumeration
Enum Status
    Pending = 0
    Active = 1
    Completed = 2
    Cancelled = 3
End Enum

' Using enumerations
Dim currentStatus As Status = Status.Active
Dim statusName As String = currentStatus.ToString()  ' "Active"
Dim statusValue As Integer = CInt(currentStatus)     ' 1

' Flags enumeration
<Flags>
Enum FilePermissions
    None = 0
    Read = 1
    Write = 2
    Execute = 4
    All = Read Or Write Or Execute
End Enum

Dim permissions As FilePermissions = FilePermissions.Read Or FilePermissions.Write
```

## Structures

```vb
' Define structure
Structure Point
    Public X As Double
    Public Y As Double
    
    Public Sub New(x As Double, y As Double)
        Me.X = x
        Me.Y = y
    End Sub
    
    Public Function DistanceFromOrigin() As Double
        Return Math.Sqrt(X * X + Y * Y)
    End Function
End Structure

' Using structures
Dim origin As New Point()  ' X = 0, Y = 0
Dim point As New Point(3.0, 4.0)
Dim distance As Double = point.DistanceFromOrigin()  ' 5.0
```

## Type Conversions

VB.NET provides several ways to convert between data types:

### Conversion Functions

| Function | Converts To | Example |
|----------|-------------|---------|
| `CBool(expression)` | Boolean | `CBool(1)` → True |
| `CByte(expression)` | Byte | `CByte(100)` → 100 |
| `CChar(expression)` | Char | `CChar("A")` → 'A'c |
| `CDate(expression)` | Date | `CDate("12/25/2024")` → #12/25/2024# |
| `CDbl(expression)` | Double | `CDbl("3.14")` → 3.14 |
| `CDec(expression)` | Decimal | `CDec("19.95")` → 19.95D |
| `CInt(expression)` | Integer | `CInt(3.14)` → 3 |
| `CLng(expression)` | Long | `CLng("1000000")` → 1000000L |
| `CObj(expression)` | Object | `CObj(42)` → Object containing 42 |
| `CSByte(expression)` | SByte | `CSByte(-100)` → -100 |
| `CShort(expression)` | Short | `CShort("1000")` → 1000S |
| `CSng(expression)` | Single | `CSng(3.14159)` → 3.14159F |
| `CStr(expression)` | String | `CStr(42)` → "42" |
| `CUInt(expression)` | UInteger | `CUInt(1000)` → 1000UI |
| `CULng(expression)` | ULong | `CULng("1000000")` → 1000000UL |
| `CUShort(expression)` | UShort | `CUShort(1000)` → 1000US |

### Conversion Examples

```vb
' Numeric conversions
Dim intValue As Integer = 42
Dim doubleValue As Double = CDbl(intValue)      ' 42.0
Dim stringValue As String = CStr(intValue)      ' "42"

' String to numeric
Dim numberString As String = "123.45"
Dim number As Double = CDbl(numberString)       ' 123.45
Dim rounded As Integer = CInt(numberString)     ' 123 (truncated)

' Boolean conversions
Dim boolValue As Boolean = CBool(1)             ' True
Dim boolString As String = CStr(True)           ' "True"

' Date conversions
Dim dateString As String = "12/25/2024"
Dim dateValue As Date = CDate(dateString)
Dim dateStr As String = dateValue.ToString("yyyy-MM-dd")  ' "2024-12-25"

' Safe conversions with TryParse
Dim input As String = "123"
Dim result As Integer
If Integer.TryParse(input, result) Then
    Console.WriteLine($"Parsed value: {result}")
Else
    Console.WriteLine("Invalid input")
End If
```

### DirectCast vs CType

```vb
' DirectCast - faster, but requires exact type match
Dim obj As Object = "Hello"
Dim str As String = DirectCast(obj, String)  ' Works

' CType - more flexible, performs conversions
Dim num As Object = 42
Dim dbl As Double = CType(num, Double)  ' Works, converts Integer to Double

' TryCast - returns Nothing if cast fails (reference types only)
Dim someObj As Object = "Test"
Dim maybeString As String = TryCast(someObj, String)
If maybeString IsNot Nothing Then
    Console.WriteLine(maybeString)
End If
```

## Type Checking

```vb
' Check type with TypeOf...Is
Dim value As Object = 42

If TypeOf value Is Integer Then
    Console.WriteLine("It's an Integer")
ElseIf TypeOf value Is String Then
    Console.WriteLine("It's a String")
End If

' Get type information
Dim valueType As Type = value.GetType()
Console.WriteLine($"Type name: {valueType.Name}")
Console.WriteLine($"Full name: {valueType.FullName}")
Console.WriteLine($"Is value type: {valueType.IsValueType}")
```

## Best Practices

1. **Use appropriate types**: Choose the smallest type that can hold your data
2. **Avoid unnecessary conversions**: They can impact performance
3. **Use TryParse for user input**: Safer than direct conversion functions
4. **Enable Option Strict**: Catches type conversion issues at compile time
5. **Be careful with floating-point**: Decimal for money, Double for scientific
6. **Use nullable types**: When a value might be missing
7. **Prefer strong typing**: Avoid Object type unless necessary

## Common Pitfalls

```vb
' Integer division vs floating-point division
Dim result1 As Double = 5 / 2      ' 2.5 (floating-point division)
Dim result2 As Integer = 5 \ 2     ' 2 (integer division)

' Overflow without checking
' This will overflow if Option Strict is Off
Dim small As Byte = 200
' Dim overflow As Byte = small + 100  ' Error! Result is 300, max is 255

' Loss of precision
Dim precise As Decimal = 123.456789012345678901234567890D
Dim imprecise As Double = CDbl(precise)  ' Loses precision

' Null vs Nothing vs Empty
Dim str1 As String = Nothing    ' Null reference
Dim str2 As String = ""         ' Empty string
Dim str3 As String = String.Empty  ' Empty string (preferred)
```

