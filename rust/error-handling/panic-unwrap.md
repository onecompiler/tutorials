# Panic and Unwrap in Rust

Understanding when to panic and how to use unwrap safely is crucial for writing robust Rust code.

## Understanding Panic

Panic is Rust's mechanism for handling unrecoverable errors. When a panic occurs, the program prints an error message, unwinds the stack, and exits.

### When Panic Occurs
```rust
fn main() {
    // Explicit panic
    panic!("Something went terribly wrong!");
    
    // This line never executes
    println!("This won't print");
}
```

### Common Panic Scenarios
```rust
fn main() {
    // Array index out of bounds
    let v = vec![1, 2, 3];
    // v[10]; // Panic!
    
    // Integer overflow in debug mode
    let x: u8 = 255;
    // let y = x + 1; // Panic in debug mode!
    
    // Unwrap on None/Err
    let x: Option<i32> = None;
    // x.unwrap(); // Panic!
}
```

## The unwrap() Method

`unwrap()` extracts the value from `Option` or `Result`, panicking if it's `None` or `Err`.

### Option::unwrap()
```rust
fn main() {
    // Safe unwrap - we know it's Some
    let x = Some(5);
    let value = x.unwrap();
    println!("Value: {}", value);
    
    // Dangerous unwrap
    let y: Option<i32> = None;
    // let value = y.unwrap(); // Panic: "called Option::unwrap() on a None value"
}
```

### Result::unwrap()
```rust
use std::fs::File;

fn main() {
    // This might panic
    // let file = File::open("nonexistent.txt").unwrap();
    
    // Safe when you're certain
    let result: Result<i32, &str> = Ok(42);
    let value = result.unwrap(); // Safe because we know it's Ok
}
```

## expect() - Unwrap with Message

`expect()` is like `unwrap()` but with a custom panic message:

```rust
use std::fs::File;

fn main() {
    // More informative panic message
    let file = File::open("config.txt")
        .expect("Config file should be present");
    
    let age: Option<u8> = None;
    // let age = age.expect("Age must be provided"); // Better error message
}
```

## When to Use Panic

### 1. Prototyping and Examples
```rust
fn main() {
    // Quick prototype - error handling can come later
    let contents = std::fs::read_to_string("data.txt").unwrap();
    let number: i32 = contents.trim().parse().unwrap();
    println!("Number: {}", number);
}
```

### 2. Tests
```rust
#[cfg(test)]
mod tests {
    #[test]
    fn test_parsing() {
        let result = "42".parse::<i32>().unwrap(); // OK in tests
        assert_eq!(result, 42);
    }
    
    #[test]
    #[should_panic(expected = "divide by zero")]
    fn test_division_panic() {
        divide(10, 0); // Should panic
    }
    
    fn divide(a: i32, b: i32) -> i32 {
        if b == 0 {
            panic!("divide by zero");
        }
        a / b
    }
}
```

### 3. Impossible Situations
```rust
fn process_validated_input(input: &str) {
    // We've already validated this can't fail
    let number: u32 = input.parse()
        .expect("Input was pre-validated");
    
    // Or use unreachable!
    match number {
        0..=100 => println!("In range"),
        _ => unreachable!("Number was validated to be 0-100"),
    }
}
```

## Safe Alternatives to unwrap()

### unwrap_or()
```rust
fn main() {
    let x: Option<i32> = None;
    let value = x.unwrap_or(0); // Default value instead of panic
    println!("Value: {}", value); // 0
    
    let result: Result<i32, &str> = Err("error");
    let value = result.unwrap_or(-1);
    println!("Value: {}", value); // -1
}
```

### unwrap_or_else()
```rust
fn main() {
    let x: Option<i32> = None;
    let value = x.unwrap_or_else(|| {
        println!("Computing default value");
        42
    });
    
    let path = "data.txt";
    let contents = std::fs::read_to_string(path)
        .unwrap_or_else(|e| {
            eprintln!("Failed to read {}: {}", path, e);
            String::from("default content")
        });
}
```

### unwrap_or_default()
```rust
fn main() {
    let x: Option<String> = None;
    let value = x.unwrap_or_default(); // Empty string
    
    let numbers: Option<Vec<i32>> = None;
    let value = numbers.unwrap_or_default(); // Empty vector
}
```

## Pattern Matching Instead of Unwrap

### Match Expression
```rust
fn safe_divide(a: f64, b: f64) -> f64 {
    if b == 0.0 {
        panic!("Attempt to divide by zero");
    }
    a / b
}

fn better_divide(a: f64, b: f64) -> Option<f64> {
    if b == 0.0 {
        None
    } else {
        Some(a / b)
    }
}

fn main() {
    match better_divide(10.0, 2.0) {
        Some(result) => println!("Result: {}", result),
        None => println!("Cannot divide by zero"),
    }
}
```

### if let
```rust
fn main() {
    let config: Option<String> = load_config();
    
    if let Some(cfg) = config {
        println!("Config: {}", cfg);
    } else {
        println!("Using default configuration");
    }
}

fn load_config() -> Option<String> {
    // Simulated config loading
    None
}
```

## Panic Safety

### catch_unwind
```rust
use std::panic;

fn main() {
    let result = panic::catch_unwind(|| {
        println!("About to panic");
        panic!("Oops!");
    });
    
    match result {
        Ok(_) => println!("No panic occurred"),
        Err(_) => println!("Caught a panic!"),
    }
    
    println!("Program continues");
}
```

### set_hook
```rust
use std::panic;

fn main() {
    panic::set_hook(Box::new(|info| {
        eprintln!("Custom panic handler: {}", info);
    }));
    
    // This will use our custom handler
    // panic!("Test panic");
}
```

## Debug vs Release Behavior

```rust
fn main() {
    // Integer overflow
    let x: u8 = 255;
    
    // Debug mode: panic
    // Release mode: wrapping
    // let y = x + 1;
    
    // Explicit wrapping
    let y = x.wrapping_add(1);
    println!("y = {}", y); // 0
    
    // Checked arithmetic
    match x.checked_add(1) {
        Some(result) => println!("Result: {}", result),
        None => println!("Overflow would occur"),
    }
}
```

## Best Practices

### 1. Use expect() for Better Error Messages
```rust
// Bad
let file = std::fs::File::open("data.txt").unwrap();

// Good
let file = std::fs::File::open("data.txt")
    .expect("data.txt should exist in current directory");
```

### 2. Document Panic Conditions
```rust
/// Divides two numbers.
/// 
/// # Panics
/// 
/// Panics if `b` is zero.
fn divide(a: i32, b: i32) -> i32 {
    if b == 0 {
        panic!("Attempt to divide by zero");
    }
    a / b
}
```

### 3. Prefer Returning Result
```rust
// Bad: panics on error
fn read_number(s: &str) -> i32 {
    s.parse().unwrap()
}

// Good: returns Result
fn read_number_safe(s: &str) -> Result<i32, std::num::ParseIntError> {
    s.parse()
}
```

### 4. Use unwrap() Only When Certain
```rust
fn main() {
    // OK: We just created it
    let v = vec![1, 2, 3];
    let first = v.get(0).unwrap();
    
    // OK: We validated the regex at compile time
    let re = regex::Regex::new(r"^\d+$").unwrap();
}
```

## Guidelines

1. **In application code**: Handle errors gracefully with Result
2. **In tests**: unwrap() and expect() are fine
3. **In examples**: unwrap() for simplicity is acceptable
4. **In libraries**: Never panic on invalid input - return Result
5. **For bugs**: panic! is appropriate for logic errors
6. **Document all panic conditions** in public APIs