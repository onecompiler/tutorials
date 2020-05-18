Structures are used to encapsulate data and related functionality. 

# How to define a structure

`struct` keyword is used to define a structure. 

```c
struct structure_name {

   member definition;
   member definition;
   ...
   member definition;
}; 
```

# How to declare structure variables

Structure variables can be declared as below:

```c
structure_name variable name;
```

## Example 

```c#
// structure definition
struct mobile {
    char model[30];
    char brand[30];
    int cost; 
}

// Declaring structure variables
mobile m1, m2;
```

## How to access structure members

You can access the structure member using `variable_name.membername`


### Example 
```c#
using System;

struct mobile {
    public string model;
    public string brand;
    public int cost; 
};

namespace Structures
{
	public class Program
	{
		public static void Main(string[] args)
		{
			  mobile m1;
        m1.brand = "Apple";

        // display the value of structure variable and accessing structure variable - brand
        Console.WriteLine("Brand of the mobile: {0} " , m1.brand);
		}
	}
}
```
### Check result [here](https://onecompiler.com/csharp/3vtbtbquh)

