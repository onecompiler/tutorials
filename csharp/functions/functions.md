Function is a sub-routine which contains set of statements. Usually functions are written when multiple calls are required to same set of statements which increases re-usuability and modularity.

## How to define a Function

```c#
<AccessSpecifier> <return-type> FunctionName(<parameters>)  
{  
//code
}
```
AccessSpecifier and parameters are optional.

## How to call a Function

```c#
function_name (parameters);
```

## Different ways of calling a user-defined functions

You can call functions in different ways as given below based on your requirement.

1. Function with no arguments and no return value.
2. Function with no arguments and a return value.
3. Function with arguments and no return value.
4. Function with arguments and a return value.

Let's look into examples for the above types.

## 1. Function with no arguments and no return value.

```c#
using System;

namespace Functions
{
	public class Program
	{
		public static void Main(string[] args)
		{
			greetings();
		}

		static void greetings()  
    {  
        Console.WriteLine("Hello world!!");  
    }
	}
}  
```
### check result [here](https://onecompiler.com/csharp/3vtdjxab6)

## 2. Function with no arguments and a return value.

```c#
using System;

namespace Functions
{
	public class Program
	{
		public static void Main(string[] args)
		{
			int res = sum();
			Console.WriteLine("sum is: {0}", res);
		}

  	static int sum() {
      int x, y, sum;
      x = 10;
      y = 20;
 
      sum = x + y;
      return (sum); // returning sum value
    }
	}
}
```
### check result [here](https://onecompiler.com/csharp/3vtdk3yu8)

## 3. Function with arguments and no return value.

```c#
using System;

namespace Functions
{
	public class Program
	{
		public static void Main(string[] args)
		{
			int x= 10, y = 20;
      sum(x,y); // passing arguments to function sum
		}

  	static int sum(int x, int y) {
       int sum;
       sum = x + y;
       Console.WriteLine("Sum : {0}", sum);
       return 0;
  	}  
	}
}
```

### Check result [here](https://onecompiler.com/csharp/3vtdkb2vu)

## 4. Function with arguments and a return value.

```c#
#include <stdio.h>
int sum();
int main()
{
  int x= 10, y = 20;
  int result;
  
  result = sum(x,y); // passing arguments to function sum
  printf("Sum : %d", result);

  
}

int sum(int x , int y) {
   int sum;
   sum = x + y;
   return sum; // returning sum value
}
```
### Check result [here](https://onecompiler.com/csharp/3vtdkhpuq)