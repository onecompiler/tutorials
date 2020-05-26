Procedure is a sub-routine which contains set of statements. Usually Procedures are written when multiple calls are required to same set of statements which increases re-usuability and modularity.

Procedures are of two types.

## 1. Functions

Functions return a value when they are called.

```vb
[accessModifiers] Function functionName [(parameterList)] As returnType
   'code
End Function
```
### Example

```c#
Public Module Program
	Public Sub Main(args() As string)
	    Dim x As Integer = 10
      Dim y As Integer = 20
      
      Dim res As Integer
      
      res = sum(x,y)
      Console.WriteLine("Sum : {0}", res)
      Console.ReadLine()
  End Sub
	
	Function Sum(ByVal a As Integer, ByVal b As Integer) As Integer
      
      Dim result As Integer
      
      result = a + b
      Sum = result
   End Function
   
End Module
```
### Check Result [here](https://onecompiler.com/vb/3vu555dhn)

## 2. Sub-Procedures

Sub-procedures are similar to functions but they don't return any value.

```vb
Sub ProcedureName (parameterList)
'Code
End Sub
```
### Example

```c#
Public Module Program
	Public Sub Main(args() As string)

      greetings("Foo")

  End Sub
	
	Sub greetings(ByVal name As String)
      
     Console.WriteLine("Hello {0}", name)
     
   End Sub
   
End Module

```
### Check Result [here](https://onecompiler.com/vb/3vu55ehzz) 