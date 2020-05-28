## 1. For..Next

For-Next loop is used to iterate a set of statements based on a condition. Usually for loop is preferred when number of iterations are known in advance.

### Syntax

```vb
For counter [ As datatype ] = begin To end [ Step step ]
   'code
   [ Continue For ]
   'code
   [ Exit For ]
   'code
Next [ counter ]
```
### Example

```c#
Public Module Program
	Public Sub Main(args() As string)
	  Dim i As Integer
  
      For i = 1 To 10 Step 1 
          Console.WriteLine(i)
      Next
      Console.ReadLine()
   End Sub
End Module	
```

### Check Result [here](https://onecompiler.com/vb/3vu29wb6a)

## 2. For..Each

For-Each loop is used to iterate a block of code for each element in a collection.

### Syntax

```vb
For Each element [ As datatype ] In group
   'code
   [ Continue For ]
   'code
   [ Exit For ]
   'code
Next [ element ]
```
### Example

```c#
Public Module Program
	Public Sub Main(args() As string)
		  Dim num() As Integer = {1, 2, 3, 4, 5}
      Dim i As Integer
      
      For Each i In num
         Console.WriteLine(i)
      Next
      Console.ReadLine()
	End Sub
End Module
```

### Check Result [here](https://onecompiler.com/vb/3vu2aa8sb)

## 3. While

While is also used to iterate a set of statements based on a condition. Usually while is preferred when number of iterations is not known in advance.

### Syntax

```vb
While conditional-expression
   'Code 
   [ Continue While ]
   'Code
   [ Exit While ]
   'Code
End While
```
### Example

```c#
Public Module Program
	Public Sub Main(args() As string)
		  Dim i As Integer = 1
      
      While i <= 10
         Console.WriteLine(i)
         i = i + 1
      End While
      Console.ReadLine()
	End Sub
End Module
```
### Check result [here](https://onecompiler.com/vb/3vu2arn75)

## 4. Do-while

Do-while is also used to iterate a set of statements based on a condition. It is mostly used when you need to execute the statements atleast once.

### Syntax

```vb
Do { While | Until } conditional-expression
   'Code
   [ Continue Do ]
   'Code
   [ Exit Do ]
   'Code
Loop
```
```vb
Do
   'Code
   [ Continue Do ]
   'Code
   [ Exit Do ]
   'Code
Loop { While | Until } conditional-expression
```
### Example

```c#
Public Module Program
	Public Sub Main(args() As string)
		  Dim i As Integer = 1
      
      Do
          Console.WriteLine(i)
          i = i + 1
      Loop While (i <= 10)
      Console.ReadLine()
	End Sub
End Module
```

### Check result [here](https://onecompiler.com/vb/3vu2azut5)