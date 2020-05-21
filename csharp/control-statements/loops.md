## 1. For

For loop is used to iterate a set of statements based on a condition. Usually for loop is preferred when number of iterations are known in advance.

### Syntax

```c#
for(Initialization; Condition; Increment/decrement){  
//code  
} 
```
### Example

```c#
// print 1 to 10 numbers in C# using for loop
using System;

namespace Loops
{
	public class Loops
	{
		public static void Main(string[] args)
		{
	
		  for (int i = 1; i <= 10; i++) {
          Console.WriteLine(i);
      }
			
		}
	}
}
```

### Check Result [here](https://onecompiler.com/csharp/3vt5mbc8g)

## 2. While

While is also used to iterate a set of statements based on a condition. Usually while is preferred when number of iterations is not known in advance.

### Syntax

```c#
while(condition){  
//code 
}  
```
### Example

```c#
// print 1 to 10 numbers in C# using while loop
using System;

namespace Loops
{
	public class Loops
	{
		public static void Main(string[] args)
		{
        int i = 1;
		    while (i <= 10) {
        Console.WriteLine(i);
        i++;
        }
	  }
	}
}
```
### Check result [here](https://onecompiler.com/csharp/3vt5mfrqt)

## 3. Do-while

Do-while is also used to iterate a set of statements based on a condition. It is mostly used when you need to execute the statements atleast once.

### Syntax

```c#
do{  
//code 
} while(condition); 
```
### Example

```c#
// print 1 to 10 numbers in C# using do-while loop
using System;

namespace Loops
{
	public class Loops
	{
		public static void Main(string[] args)
		{
      int i = 1;
      do {
      Console.WriteLine(i);
      i++;
      } while(i <= 10 );
			
		}
	}
}
```

### Check result [here](https://onecompiler.com/csharp/3vt5mkn9j)