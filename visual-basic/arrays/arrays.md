Array is a collection of similar data which is stored in continuous memory addresses. Array values can be fetched using index. 

Index starts from 0 to size-1.

Arrays can be one-dimensional or multi-dimensional in VB.net language. The more popular and frequently used arrays are one-dimensional and two-dimensional arrays.

Arrays store the collection of data sequentially in memory and they must share same data type.

## How to declare an array?

```c#
Dim array-name as Datatype  = {values}
```

### Example

```c#
Dim num(10) As Integer	'an array of 11 integers

Dim matrix(4, 4) As Integer	'2-D array of integers

Dim directions() As String = {"East", "West", "North", "South"} 'An array of 4 string elements 
```

## How to access array elements

Array elements can be accessed by using indices. Array indices starts from `0`.  `Array(n-1)` can be used to access nth element of an array.

## Example

### One-Dimensional Array Example

```c#
Public Module Arrays
	Public Sub Main(args() As string)
		Dim directions() As String = {"East", "West", "North", "South"} 
    Dim i as integer = 0
    For i = 0 To 3
         Console.WriteLine(directions(i))
         Next i
    Console.ReadKey()
   End Sub
End Module
```
### Check result [here](https://onecompiler.com/vb/3vtttv828)


### Two-Dimensional Array Example

```c#
Public Module Arrays
	Public Sub Main(args() As string)
		Dim matrix( , ) As Integer = {{1,2}, {3,4},{5,6}} 
    Dim i, j as integer = 0
    
    For i = 0 To 2
      For j = 0 To 1
        Console.WriteLine("matrix[{0}, {1}] = {2}", i, j, matrix(i, j))
        Next j
      Next i     
    
    Console.ReadKey()
   End Sub
End Module
```

### Check result [here](https://onecompiler.com/vb/3vttud8gc)

## Summary

* Arrays is a collection of homogeneous data.
* Arrays stores data sequentially in memory.
* Arrays are finite.

