# Modern C# Features

Modern C# has evolved significantly with powerful features that enhance code safety, readability, and performance. Here are some of the most important modern features introduced in recent versions of C#.

## 1. Nullable Reference Types

Nullable reference types help prevent null reference exceptions by making nullability explicit in your code. Introduced in C# 8.0, this feature provides compile-time warnings when you might be dereferencing a null reference.

### Enabling Nullable Reference Types

```c#
#nullable enable

public class Person
{
    public string Name { get; set; } = string.Empty; // Non-nullable
    public string? Email { get; set; } // Nullable
    public int Age { get; set; }
}
```

### Example Usage

```c#
using System;

#nullable enable

namespace NullableExample
{
    public class Program
    {
        public static void Main(string[] args)
        {
            Person person = new Person();
            person.Name = "John Doe";
            person.Email = null; // This is allowed
            
            Console.WriteLine($"Name: {person.Name}");
            Console.WriteLine($"Email: {person.Email ?? "No email provided"}");
            
            // This would generate a warning if Email is null
            // Console.WriteLine($"Email length: {person.Email.Length}");
        }
    }
    
    public class Person
    {
        public string Name { get; set; } = string.Empty;
        public string? Email { get; set; }
        public int Age { get; set; }
    }
}
```

## 2. Records

Records provide a concise syntax for creating immutable data types with value-based equality. They're perfect for data transfer objects and value objects.

### Basic Record Syntax

```c#
public record Person(string FirstName, string LastName, int Age);
```

### Record with Custom Members

```c#
using System;

namespace RecordExample
{
    public class Program
    {
        public static void Main(string[] args)
        {
            var person1 = new Person("John", "Doe", 30);
            var person2 = new Person("John", "Doe", 30);
            var person3 = person1 with { Age = 31 }; // Non-destructive mutation
            
            Console.WriteLine($"Person 1: {person1}");
            Console.WriteLine($"Person 2: {person2}");
            Console.WriteLine($"Person 3: {person3}");
            Console.WriteLine($"Are person1 and person2 equal? {person1 == person2}");
        }
    }
    
    public record Person(string FirstName, string LastName, int Age)
    {
        public string FullName => $"{FirstName} {LastName}";
        
        public bool IsAdult => Age >= 18;
    }
}
```

## 3. Pattern Matching

Pattern matching allows you to test expressions against patterns and extract values. It's been enhanced significantly in modern C#.

### Switch Expressions

```c#
using System;

namespace PatternMatchingExample
{
    public class Program
    {
        public static void Main(string[] args)
        {
            Console.WriteLine(GetDayType(DayOfWeek.Monday));
            Console.WriteLine(GetDayType(DayOfWeek.Saturday));
            Console.WriteLine(GetDayType(DayOfWeek.Sunday));
            
            var numbers = new int[] { 1, 2, 3, 4, 5 };
            Console.WriteLine(DescribeCollection(numbers));
            
            var emptyArray = new int[] { };
            Console.WriteLine(DescribeCollection(emptyArray));
        }
        
        public static string GetDayType(DayOfWeek day) => day switch
        {
            DayOfWeek.Monday or DayOfWeek.Tuesday or DayOfWeek.Wednesday 
                or DayOfWeek.Thursday or DayOfWeek.Friday => "Weekday",
            DayOfWeek.Saturday or DayOfWeek.Sunday => "Weekend",
            _ => "Unknown"
        };
        
        public static string DescribeCollection<T>(T[] array) => array switch
        {
            { Length: 0 } => "Empty array",
            { Length: 1 } => "Single element array",
            { Length: > 1 and <= 5 } => "Small array",
            { Length: > 5 } => "Large array",
            _ => "Unknown"
        };
    }
}
```

### Type Patterns and Property Patterns

```c#
using System;

namespace AdvancedPatternMatching
{
    public class Program
    {
        public static void Main(string[] args)
        {
            var shapes = new Shape[]
            {
                new Circle(5),
                new Rectangle(4, 6),
                new Square(3)
            };
            
            foreach (var shape in shapes)
            {
                Console.WriteLine($"Area: {GetArea(shape)}");
                Console.WriteLine($"Description: {DescribeShape(shape)}");
            }
        }
        
        public static double GetArea(Shape shape) => shape switch
        {
            Circle { Radius: var r } => Math.PI * r * r,
            Rectangle { Width: var w, Height: var h } => w * h,
            Square { Side: var s } => s * s,
            _ => 0
        };
        
        public static string DescribeShape(Shape shape) => shape switch
        {
            Circle { Radius: > 10 } => "Large circle",
            Circle => "Circle",
            Rectangle { Width: var w, Height: var h } when w == h => "Square-like rectangle",
            Rectangle => "Rectangle",
            Square { Side: < 5 } => "Small square",
            Square => "Square",
            _ => "Unknown shape"
        };
    }
    
    public abstract record Shape;
    public record Circle(double Radius) : Shape;
    public record Rectangle(double Width, double Height) : Shape;
    public record Square(double Side) : Shape;
}
```

## 4. Top-Level Programs

Top-level programs eliminate the need for boilerplate code in simple applications. Available since C# 9.0.

### Traditional Program Structure

```c#
using System;

namespace TraditionalProgram
{
    public class Program
    {
        public static void Main(string[] args)
        {
            Console.WriteLine("Hello, World!");
        }
    }
}
```

### Top-Level Program (Modern)

```c#
using System;

Console.WriteLine("Hello, World!");
Console.WriteLine("This is a top-level program!");

// You can still define methods and classes
void PrintMessage(string message)
{
    Console.WriteLine($"Message: {message}");
}

PrintMessage("Top-level programs are concise!");

// Local functions and classes are also supported
public class Helper
{
    public static void DoSomething()
    {
        Console.WriteLine("Helper method called");
    }
}

Helper.DoSomething();
```

## 5. String Interpolation and Raw Strings

### String Interpolation

String interpolation provides a more readable way to format strings.

```c#
using System;

namespace StringInterpolationExample
{
    public class Program
    {
        public static void Main(string[] args)
        {
            string name = "Alice";
            int age = 30;
            double salary = 75000.50;
            
            // Basic interpolation
            Console.WriteLine($"Name: {name}, Age: {age}");
            
            // With formatting
            Console.WriteLine($"Salary: {salary:C}");
            Console.WriteLine($"Age in hex: {age:X}");
            
            // With expressions
            Console.WriteLine($"Next year, {name} will be {age + 1} years old");
            
            // Conditional expressions
            Console.WriteLine($"{name} is {(age >= 18 ? "an adult" : "a minor")}");
        }
    }
}
```

### Raw Strings (C# 11+)

Raw strings allow you to include quotes and other special characters without escaping.

```c#
using System;

namespace RawStringsExample
{
    public class Program
    {
        public static void Main(string[] args)
        {
            // Traditional string with escaping
            string json1 = "{\n  \"name\": \"John\",\n  \"age\": 30\n}";
            
            // Raw string - no escaping needed
            string json2 = """
                {
                  "name": "John",
                  "age": 30
                }
                """;
            
            // Raw string with interpolation
            string name = "Alice";
            int age = 25;
            string interpolatedJson = $$"""
                {
                  "name": "{{name}}",
                  "age": {{age}},
                  "status": "active"
                }
                """;
            
            Console.WriteLine("Traditional JSON:");
            Console.WriteLine(json1);
            Console.WriteLine("\nRaw string JSON:");
            Console.WriteLine(json2);
            Console.WriteLine("\nInterpolated raw string JSON:");
            Console.WriteLine(interpolatedJson);
            
            // Multi-line raw string for SQL
            string sql = """
                SELECT p.FirstName, p.LastName, p.Age
                FROM Person p
                WHERE p.Age > 18
                ORDER BY p.LastName, p.FirstName
                """;
            
            Console.WriteLine("\nSQL Query:");
            Console.WriteLine(sql);
        }
    }
}
```

## Summary

Modern C# features significantly improve code quality, safety, and developer productivity:

- **Nullable Reference Types** help prevent null reference exceptions
- **Records** provide immutable data types with value-based equality
- **Pattern Matching** enables powerful and expressive conditional logic
- **Top-Level Programs** reduce boilerplate code for simple applications
- **String Interpolation and Raw Strings** make string formatting more readable and maintainable

These features work together to make C# a more powerful and expressive language while maintaining backward compatibility and performance.