# Conditional Statements in Rust

Conditional statements allow your program to make decisions and execute different code paths based on conditions.

## The `if` Expression

The basic `if` expression:

```rust
fn main() {
    let number = 7;
    
    if number < 10 {
        println!("Number is less than 10");
    }
}
```

## `if-else` Expression

Add an alternative path with `else`:

```rust
fn main() {
    let age = 18;
    
    if age >= 18 {
        println!("You are an adult");
    } else {
        println!("You are a minor");
    }
}
```

## `else if` Chains

Handle multiple conditions:

```rust
fn main() {
    let score = 85;
    
    if score >= 90 {
        println!("Grade: A");
    } else if score >= 80 {
        println!("Grade: B");
    } else if score >= 70 {
        println!("Grade: C");
    } else if score >= 60 {
        println!("Grade: D");
    } else {
        println!("Grade: F");
    }
}
```

## `if` is an Expression

In Rust, `if` is an expression, not a statement, so it returns a value:

```rust
fn main() {
    let condition = true;
    let number = if condition { 5 } else { 6 };
    
    println!("The number is: {}", number);
}
```

### Important: Types Must Match
```rust
fn main() {
    let condition = true;
    
    // This won't compile - types must match
    // let number = if condition { 5 } else { "six" };
    
    // This works - both branches return the same type
    let number = if condition { 5 } else { 0 };
    println!("Number: {}", number);
}
```

## Using `if` in `let` Statements

Conditional assignment:

```rust
fn main() {
    let temperature = 25;
    
    let weather = if temperature > 30 {
        "hot"
    } else if temperature > 20 {
        "warm"
    } else if temperature > 10 {
        "cool"
    } else {
        "cold"
    };
    
    println!("The weather is {}", weather);
}
```

## Pattern Matching with `match`

The `match` expression is Rust's powerful pattern matching:

### Basic Match
```rust
fn main() {
    let number = 3;
    
    match number {
        1 => println!("One"),
        2 => println!("Two"),
        3 => println!("Three"),
        _ => println!("Other number"), // _ matches everything else
    }
}
```

### Match with Multiple Patterns
```rust
fn main() {
    let day = 6;
    
    match day {
        1..=5 => println!("Weekday"),
        6 | 7 => println!("Weekend"),
        _ => println!("Invalid day"),
    }
}
```

### Match Must Be Exhaustive
```rust
fn main() {
    let boolean = true;
    
    // Must cover all possible values
    let result = match boolean {
        true => 1,
        false => 0,
        // No need for _ here, all cases covered
    };
    
    println!("Result: {}", result);
}
```

### Match with Guards
```rust
fn main() {
    let pair = (2, -2);
    
    match pair {
        (x, y) if x == y => println!("Equal"),
        (x, y) if x + y == 0 => println!("Sum is zero"),
        (x, _) if x % 2 == 0 => println!("First is even"),
        _ => println!("No match"),
    }
}
```

## `if let` for Simple Patterns

When you only care about one pattern:

```rust
fn main() {
    let favorite_color: Option<&str> = Some("blue");
    
    // Using match
    match favorite_color {
        Some(color) => println!("Favorite color is {}", color),
        None => (),
    }
    
    // Using if let - more concise
    if let Some(color) = favorite_color {
        println!("Favorite color is {}", color);
    }
}
```

### `if let` with `else`
```rust
fn main() {
    let age: Option<u8> = None;
    
    if let Some(age) = age {
        println!("Age is {}", age);
    } else {
        println!("Age is not provided");
    }
}
```

## Practical Examples

### User Authentication
```rust
fn authenticate(username: &str, password: &str) -> bool {
    username == "admin" && password == "secure123"
}

fn main() {
    let user = "admin";
    let pass = "secure123";
    
    if authenticate(user, pass) {
        println!("Welcome, {}!", user);
    } else {
        println!("Invalid credentials");
    }
}
```

### Temperature Converter with Validation
```rust
fn main() {
    let temp_f = 75.0;
    
    let description = if temp_f > 212.0 {
        "Above boiling point"
    } else if temp_f >= 86.0 {
        "Hot"
    } else if temp_f >= 68.0 {
        "Comfortable"
    } else if temp_f >= 32.0 {
        "Cold"
    } else {
        "Below freezing"
    };
    
    println!("{}°F is {}", temp_f, description);
}
```

### Match on Enum
```rust
enum Direction {
    North,
    South,
    East,
    West,
}

fn main() {
    let direction = Direction::North;
    
    match direction {
        Direction::North => println!("Going up!"),
        Direction::South => println!("Going down!"),
        Direction::East => println!("Going right!"),
        Direction::West => println!("Going left!"),
    }
}
```

### Nested Conditions
```rust
fn main() {
    let is_weekend = true;
    let weather = "sunny";
    
    if is_weekend {
        if weather == "sunny" {
            println!("Perfect day for outdoor activities!");
        } else {
            println!("Good day for indoor hobbies!");
        }
    } else {
        println!("It's a work day");
    }
}
```

## Best Practices

1. Use `match` when checking multiple specific values
2. Use `if let` when you only care about one pattern
3. Keep conditions simple and readable
4. Consider extracting complex conditions into functions
5. Remember that `if` is an expression - use it to return values