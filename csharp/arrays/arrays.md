Array is a collection of similar data which is stored in continuous memory addresses. Array values can be fetched using index. 

Index starts from 0 to size-1.

Arrays can be one-dimensional or multi-dimensional in C# language. The more popular and frequently used arrays are one-dimensional and two-dimensional arrays.

Arrays store the collection of data sequentially in memory and they must share same data type.

## How to declare an array?

```c#
data-type[] array-name; //declaration
array-name = new data-type[size]{ array-elements }; //initialization
```
\[or\]

```c#
data-type[] array-name =  new data-type[size]{ array-elements }; //declaration and initialization
```
\[or\]

```c#
data-type[] array-name =  { array-elements }; //short syntax of array declaration and initialization
```
## Examples

```c
int[] num = {1,2,3,4,5};
```

## How to access array elements

Array elements can be accessed by using indices. Array indices starts from `0`.  `Array[n-1]` can be used to access nth element of an array.

## Example

```c#
using System;

namespace Arrays
{
	public class Arrays
	{
		public static void Main(string[] args)
		{
		    int[] arr = {1,2,3,4,5};
        int n = arr.Length; // gives length of an array
        Console.WriteLine("Length of the array: {0}" , n );
        Console.WriteLine("first element: {0}" , arr[0] ); // prints first element of the array
        Console.WriteLine( "last element: {0}" , arr[n-1]); // prints last element of the array
    }
	}
}
```
### Check result [here](https://onecompiler.com/cpp/3vmbf4ed9)

## Summary

* Arrays is a collection of homogeneous data.
* Arrays stores data sequentially in memory.
* Arrays are finite.

