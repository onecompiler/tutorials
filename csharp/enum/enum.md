Enum is a special class which is used to represent group of constants. `enum` is the keyword used to create an enum.

### Syntax

```c#
enum name{constant1, constant2, constant3, ....... } ;
```
### Example

```c#
using System;

namespace Enum
{
	public class Program
	{
	  enum directions{North, South, East, West};
			
		public static void Main(string[] args)
		{
			
			int x = (int)directions.East;
			Console.WriteLine(x);
			
			int y = (int)directions.North;
			Console.WriteLine(y);
		}
	}
}
```
### Run [here](https://onecompiler.com/csharp/3vtebgwut)
