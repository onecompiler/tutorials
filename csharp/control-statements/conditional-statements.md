When ever you want to perform a set of operations based on a condition(s) If / If-Else / Nested ifs are used.

You can also use if-else , nested Ifs and If-else-if ladder when multiple conditions are to be performed on a single variable.

## 1. If

### Syntax

```c#
if(conditional-expression)
{
    //code
}
```
### Example

```c#
using System;

namespace loops
{
	public class statement
	{
		public static void Main(string[] args)
		{
        int x = 30;
        int y = 30;

        if ( x == y) {
            Console.WriteLine("x and y are equal");
        }
		}
	}
}
```
### Check result [here](https://onecompiler.com/csharp/3vt56y88t)

## 2. If-else

### Syntax

```c#
if(conditional-expression)
{
    //code
} else {
    //code
}
```
### Example

```c#
#include <iostream>
using namespace std;

int main() 
{
  
  int x = 30;
  int y = 20;

  if ( x == y) {
    cout << "x and y are equal";
  } else {
    cout <<"x and y are not equal";  
  }
}
```
### Check result [here](https://onecompiler.com/csharp/3vt5kcn54)

## 3. If-else-if ladder

### Syntax
```c#
if(conditional-expression-1)
{
    //code
} else if(conditional-expression-2) {
    //code
} else if(conditional-expression-3) {
    //code
}
....
else {
    //code
}
```

### Example
```c#
using System;

namespace loops
{
	public class statement
	{
	public static void Main(string[] args)
		{
	     int age = 15;

       if ( age <= 1 && age >= 0) {
           Console.WriteLine( "Infant");
       } else if (age > 1 && age <= 3) {
           Console.WriteLine( "Toddler");
       } else if (age > 3 && age <= 9) {
           Console.WriteLine( "Child");
       } else if (age > 9 && age <= 18) {
           Console.WriteLine( "Teen");
       } else if (age > 18) {
           Console.WriteLine( "Adult");
       } else {
           Console.WriteLine( "Invalid Age");
       }

		}
	}
}
```
### Check result [here](https://onecompiler.com/csharp/3vt5kkj2d)

## 4. Nested-If

Nested-Ifs represents if block within another if block. 

### Syntax
```c#
if(conditional-expression-1) {    
     //code    
          if(conditional-expression-2) {  
             //code
             if(conditional-expression-3) {
                 //code
             }  
    }    
}
```

### Example
```c#
using System;

namespace loops
{
	public class statement
	{
	public static void Main(string[] args)
		{
	      int age = 50;
        char resident = 'Y';
        if (age > 18)
        {
          if(resident == 'Y'){
            Console.WriteLine("Eligible to Vote");
          }
        }
		}
	}
}
```
### Check result [here](https://onecompiler.com/csharp/3vt5kvgkd)

## 5. Switch

Switch is an alternative to If-Else-If ladder and to select one among many blocks of code.

### Syntax

```c#
switch(conditional-expression){    
case value1:    
 //code    
 break;  //optional  
case value2:    
 //code    
 break;  //optional  
...    
    
default:     
 //code to be executed when all the above cases are not matched;    
} 
```
### Example
```c#
using System;

namespace loops
{
	public class statement
	{
	public static void Main(string[] args)
		{
      int day = 5;
       
      switch(day){
         case 1: Console.WriteLine("Sunday");
         break;
         case 2: Console.WriteLine("Monday");
         break;
         case 3: Console.WriteLine("Tuesday");
         break;
         case 4: Console.WriteLine("Wednesday");
         break;
         case 5: Console.WriteLine("Thursday");
         break;
         case 6: Console.WriteLine("Friday");
         break;
         case 7: Console.WriteLine("Saturday");
         break;
         default: Console.WriteLine("Invalid day");
         break; 
      }
		}
	}
}
```
###  Check Result [here](https://onecompiler.com/csharp/3vt5m5f4q)
