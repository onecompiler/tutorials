When ever you want to perform a set of operations based on a condition(s) If / If-Else / Nested ifs are used.

You can also use if-else , nested Ifs and If-else-if ladder when multiple conditions are to be performed on a single variable.

## 1. If

### Syntax

```vb
If condition-expression Then 
    'code
End If
```
### Example

```c#
Public Module Program
	Public Sub Main(args() As string)
		Dim x as integer= 30
    Dim y as integer = 30

    If ( x = y) Then
        Console.WriteLine("x and y are equal")
    End If
		
	End Sub
End Module
```
### Check result [here](https://onecompiler.com/vb/3vtymuwva)

## 2. If-else

### Syntax

```vb
If(conditional-expression)Then
   'code if the conditional-expression is true 
Else
  'code if the conditional-expression is false 
End If
```
### Example

```c#
Public Module Program
	Public Sub Main(args() As string)
		Dim x as integer= 30
    Dim y as integer = 10

    If ( x = y) Then
        Console.WriteLine("x and y are equal")
    else
         Console.WriteLine("x and y are not equal")
    End If
		
	End Sub
End Module
```
### Check result [here](https://onecompiler.com/vb/3vtynbxft)

## 3. If-else-if ladder

### Syntax
```vb
If(conditional-expression)Then
   'code if the above conditional-expression is true 
Else If(conditional-expression) Then
        'code if the above conditional-expression is true 
    Else
        'code if the above conditional-expression is false 
End If
```

### Example
```c#
Public Module Program
	Public Sub Main(args() As string)
		Dim x as integer= 30
    Dim y as integer = 100

    If ( x = y) Then
         Console.WriteLine("x and y are equal")
    else if (x > y) Then
            Console.WriteLine("x is greater than y")
         else
            Console.WriteLine("y is greater than x")
    End If
		
	End Sub
End Module
```
### Check result [here](https://onecompiler.com/vb/3vtynejmt)

## 4. Nested-If

Nested-Ifs represents if block within another if block. 

### Syntax
```vb
If(conditional-expression)Then
   'code if the above conditional-expression is true
   If(conditional-expression)Then
         'code if the above conditional-expression is true 
   End If
End If
```

### Example
```c#
Public Module Program
	Public Sub Main(args() As string)
		Dim age as integer= 30
    Dim resident as char = "Y"

    If (age > 18) Then
        If(resident = "Y") Then
            Console.WriteLine("Eligible to Vote")
        End If
    End If

	End Sub
End Module
```
### Check result [here](https://onecompiler.com/vb/3vtyyzxwd)

## 5. Select Case

Select Case is an alternative to If-Else-If ladder and to select one among many blocks of code.

### Syntax

```vb
Select [ Case ] expression
   [ Case expressionlist
      'code ]
   [ Case Else
      'code ]
End Select
```
### Example
```c#
Public Module Program
	Public Sub Main(args() As string)
		  Dim directions As Char = "E"
      
      Select directions
          Case "N"
              Console.WriteLine("North")
          Case "S"
              Console.WriteLine("South")
          Case "E"
              Console.WriteLine("East")
          Case "W"
              Console.WriteLine("West")
          Case Else
              Console.WriteLine("Invalid direction")
      End Select
      
	End Sub
End Module
```
###  Check Result [here](https://onecompiler.com/vb/3vtz493mc)
