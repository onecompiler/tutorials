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
using System;

namespace loops
{
    public class statement
    {
        public static void Main(string[] args)
        {
            int x = 30;
            int y = 20;

            if (x == y)
            {
                Console.WriteLine("x and y are equal");
            }
            else
            {
                Console.WriteLine("x and y are not equal");
            }
        }
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

## 6. Switch Expression (C# 8.0+)

Switch expressions provide a more concise syntax for switch statements when you need to return a value based on a pattern match.

### Syntax

```c#
var result = conditional-expression switch
{
    pattern1 => value1,
    pattern2 => value2,
    pattern3 when condition => value3,
    _ => defaultValue
};
```

### Example 1: Basic Switch Expression

```c#
using System;

namespace ConditionalStatements
{
    public class SwitchExpressionExample
    {
        public static void Main(string[] args)
        {
            int day = 3;
            
            string dayName = day switch
            {
                1 => "Sunday",
                2 => "Monday",
                3 => "Tuesday",
                4 => "Wednesday",
                5 => "Thursday",
                6 => "Friday",
                7 => "Saturday",
                _ => "Invalid day"
            };
            
            Console.WriteLine($"Day {day} is {dayName}");
        }
    }
}
```

### Example 2: Switch Expression with When Clause

```c#
using System;

namespace ConditionalStatements
{
    public class SwitchExpressionWithWhen
    {
        public static void Main(string[] args)
        {
            int score = 85;
            
            string grade = score switch
            {
                >= 90 => "A",
                >= 80 => "B",
                >= 70 => "C",
                >= 60 => "D",
                _ => "F"
            };
            
            Console.WriteLine($"Score: {score}, Grade: {grade}");
            
            // With when clause for more complex conditions
            int age = 25;
            bool isStudent = true;
            
            string ticketPrice = (age, isStudent) switch
            {
                (< 18, _) => "Child: $10",
                (_, true) => "Student: $15",
                (>= 65, false) => "Senior: $12",
                _ => "Adult: $20"
            };
            
            Console.WriteLine($"Ticket price: {ticketPrice}");
        }
    }
}
```

### Example 3: Pattern Matching with Switch Expression

```c#
using System;

namespace ConditionalStatements
{
    public class PatternMatchingExample
    {
        public static void Main(string[] args)
        {
            object value = 42;
            
            string result = value switch
            {
                int n when n > 0 => $"Positive integer: {n}",
                int n when n < 0 => $"Negative integer: {n}",
                int => "Zero",
                string s => $"String value: {s}",
                null => "Null value",
                _ => "Unknown type"
            };
            
            Console.WriteLine(result);
        }
    }
}
```
