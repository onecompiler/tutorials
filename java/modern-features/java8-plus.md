# Modern Java Features (Java 8+)

Java 8 introduced several revolutionary features that changed how developers write Java code. This guide covers the most important modern Java features that every developer should know.

## Lambda Expressions

Lambda expressions allow you to treat functionality as a method argument, or treat code as data. They provide a clear and concise way to represent one method interface using an expression.

### Syntax
```java
(parameters) -> expression
// or
(parameters) -> { statements; }
```

### Example
```java
import java.util.*;

public class LambdaExample {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David");
        
        // Without lambda (traditional approach)
        names.sort(new Comparator<String>() {
            @Override
            public int compare(String a, String b) {
                return a.compareTo(b);
            }
        });
        
        // With lambda expression
        names.sort((a, b) -> a.compareTo(b));
        
        // Even simpler with method reference
        names.sort(String::compareTo);
        
        System.out.println(names);
    }
}
```

### Check output [here](https://onecompiler.com/java)

## Stream API

The Stream API provides a powerful and flexible way to process collections of data. It allows you to perform complex data processing operations in a declarative way.

### Common Stream Operations
- `filter()` - Filter elements based on a condition
- `map()` - Transform elements
- `collect()` - Collect results into a collection
- `forEach()` - Perform an action on each element
- `reduce()` - Reduce elements to a single value

### Example
```java
import java.util.*;
import java.util.stream.Collectors;

public class StreamExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        
        // Filter even numbers, multiply by 2, and collect to list
        List<Integer> result = numbers.stream()
            .filter(n -> n % 2 == 0)
            .map(n -> n * 2)
            .collect(Collectors.toList());
        
        System.out.println("Even numbers doubled: " + result);
        
        // Find sum of all numbers
        int sum = numbers.stream()
            .reduce(0, Integer::sum);
        
        System.out.println("Sum: " + sum);
        
        // Count elements greater than 5
        long count = numbers.stream()
            .filter(n -> n > 5)
            .count();
        
        System.out.println("Count > 5: " + count);
    }
}
```

### Check output [here](https://onecompiler.com/java)

## Optional Class

Optional is a container object which may or may not contain a non-null value. It helps avoid NullPointerException and makes your code more readable.

### Example
```java
import java.util.Optional;

public class OptionalExample {
    public static void main(String[] args) {
        // Creating Optional objects
        Optional<String> optional1 = Optional.of("Hello");
        Optional<String> optional2 = Optional.empty();
        Optional<String> optional3 = Optional.ofNullable(null);
        
        // Checking if value is present
        if (optional1.isPresent()) {
            System.out.println("Value: " + optional1.get());
        }
        
        // Using orElse for default values
        String value = optional2.orElse("Default Value");
        System.out.println("Value with default: " + value);
        
        // Using ifPresent for actions
        optional1.ifPresent(v -> System.out.println("Processing: " + v));
        
        // Chaining operations
        Optional<String> result = optional1
            .map(String::toUpperCase)
            .filter(s -> s.length() > 3);
        
        result.ifPresent(System.out::println);
    }
}
```

### Check output [here](https://onecompiler.com/java)

## Method References

Method references provide a way to refer to methods without executing them. They are used to make lambda expressions more readable when they only call an existing method.

### Types of Method References
1. **Static method reference** - `ClassName::staticMethod`
2. **Instance method reference** - `instance::instanceMethod`
3. **Constructor reference** - `ClassName::new`
4. **Arbitrary object method reference** - `ClassName::instanceMethod`

### Example
```java
import java.util.*;
import java.util.function.Function;

public class MethodReferenceExample {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("alice", "bob", "charlie");
        
        // Static method reference
        names.sort(String::compareToIgnoreCase);
        System.out.println("Sorted: " + names);
        
        // Instance method reference
        names.forEach(System.out::println);
        
        // Constructor reference
        Function<String, String> stringConstructor = String::new;
        String newString = stringConstructor.apply("Hello");
        System.out.println("New string: " + newString);
        
        // Converting to uppercase using method reference
        names.stream()
            .map(String::toUpperCase)
            .forEach(System.out::println);
    }
}
```

### Check output [here](https://onecompiler.com/java)

## Default Methods in Interfaces

Default methods allow you to add new methods to interfaces without breaking existing implementations. This feature enables interface evolution.

### Example
```java
interface Vehicle {
    // Abstract method
    void start();
    
    // Default method
    default void stop() {
        System.out.println("Vehicle is stopping...");
    }
    
    // Static method
    static void checkMaintenanceSchedule() {
        System.out.println("Checking maintenance schedule...");
    }
}

class Car implements Vehicle {
    @Override
    public void start() {
        System.out.println("Car is starting...");
    }
    
    // Can override default method if needed
    @Override
    public void stop() {
        System.out.println("Car is stopping with brakes...");
    }
}

class Bike implements Vehicle {
    @Override
    public void start() {
        System.out.println("Bike is starting...");
    }
    
    // Uses default implementation of stop()
}

public class DefaultMethodExample {
    public static void main(String[] args) {
        Car car = new Car();
        car.start();
        car.stop();
        
        Bike bike = new Bike();
        bike.start();
        bike.stop(); // Uses default implementation
        
        Vehicle.checkMaintenanceSchedule(); // Static method call
    }
}
```

### Check output [here](https://onecompiler.com/java)

## Combining Modern Features

Here's an example that combines multiple modern Java features:

```java
import java.util.*;
import java.util.stream.Collectors;

class Person {
    private String name;
    private int age;
    private String city;
    
    public Person(String name, int age, String city) {
        this.name = name;
        this.age = age;
        this.city = city;
    }
    
    // Getters
    public String getName() { return name; }
    public int getAge() { return age; }
    public String getCity() { return city; }
    
    @Override
    public String toString() {
        return name + " (" + age + ") from " + city;
    }
}

public class ModernJavaExample {
    public static void main(String[] args) {
        List<Person> people = Arrays.asList(
            new Person("Alice", 30, "New York"),
            new Person("Bob", 25, "London"),
            new Person("Charlie", 35, "New York"),
            new Person("David", 28, "London")
        );
        
        // Find people over 27 from New York, sorted by name
        List<Person> result = people.stream()
            .filter(p -> p.getAge() > 27)
            .filter(p -> "New York".equals(p.getCity()))
            .sorted(Comparator.comparing(Person::getName))
            .collect(Collectors.toList());
        
        System.out.println("People over 27 from New York:");
        result.forEach(System.out::println);
        
        // Group people by city
        Map<String, List<Person>> peopleByCity = people.stream()
            .collect(Collectors.groupingBy(Person::getCity));
        
        System.out.println("\nPeople grouped by city:");
        peopleByCity.forEach((city, persons) -> {
            System.out.println(city + ": " + persons);
        });
        
        // Find average age using Optional
        OptionalDouble averageAge = people.stream()
            .mapToInt(Person::getAge)
            .average();
        
        averageAge.ifPresent(avg -> 
            System.out.println("\nAverage age: " + avg));
    }
}
```

### Check output [here](https://onecompiler.com/java)

## Benefits of Modern Java Features

1. **More Readable Code** - Lambda expressions and method references make code more concise
2. **Functional Programming** - Stream API enables functional programming paradigms
3. **Better Null Safety** - Optional class helps prevent NullPointerException
4. **Interface Evolution** - Default methods allow interfaces to evolve without breaking implementations
5. **Better Performance** - Stream operations can be optimized by the JVM
6. **Parallel Processing** - Streams can be easily parallelized for better performance

These modern features have made Java more expressive, safer, and more efficient while maintaining backward compatibility.