# Option and Result in Rust

`Option` and `Result` are two of Rust's most important enums, used for handling nullable values and errors respectively.

## The Option Enum

Option represents an optional value: either `Some` value or `None`.

```rust
enum Option<T> {
    Some(T),
    None,
}
```

### Basic Usage
```rust
fn main() {
    let some_number: Option<i32> = Some(5);
    let no_number: Option<i32> = None;
    
    // Pattern matching
    match some_number {
        Some(n) => println!("Got number: {}", n),
        None => println!("No number"),
    }
}
```

### Common Option Methods

#### `unwrap()` and `expect()`
```rust
fn main() {
    let x = Some(5);
    let y: Option<i32> = None;
    
    println!("x: {}", x.unwrap()); // 5
    // y.unwrap(); // This would panic!
    
    // expect() with custom error message
    let z = Some(10);
    println!("z: {}", z.expect("Should have a value"));
}
```

#### `unwrap_or()` and `unwrap_or_else()`
```rust
fn main() {
    let x: Option<i32> = None;
    
    // Provide default value
    println!("x: {}", x.unwrap_or(0)); // 0
    
    // Compute default value
    println!("x: {}", x.unwrap_or_else(|| {
        println!("Computing default...");
        42
    }));
}
```

#### `map()` and `map_or()`
```rust
fn main() {
    let maybe_number = Some(5);
    
    // Transform the value inside
    let maybe_string = maybe_number.map(|n| n.to_string());
    println!("{:?}", maybe_string); // Some("5")
    
    // Map with default
    let x: Option<i32> = None;
    let result = x.map_or(0, |n| n * 2);
    println!("Result: {}", result); // 0
}
```

#### `and_then()` (flatMap)
```rust
fn square(x: u32) -> Option<u32> {
    Some(x * x)
}

fn main() {
    let number = Some(2);
    let result = number.and_then(square).and_then(square);
    println!("{:?}", result); // Some(16)
    
    let none: Option<u32> = None;
    let result = none.and_then(square);
    println!("{:?}", result); // None
}
```

### Practical Option Examples

#### Finding Values
```rust
fn find_user(id: u32) -> Option<String> {
    let users = vec![
        (1, "Alice"),
        (2, "Bob"),
        (3, "Charlie"),
    ];
    
    users.into_iter()
        .find(|(user_id, _)| *user_id == id)
        .map(|(_, name)| name.to_string())
}

fn main() {
    match find_user(2) {
        Some(name) => println!("Found user: {}", name),
        None => println!("User not found"),
    }
}
```

#### Configuration Values
```rust
struct Config {
    timeout: Option<u64>,
    retries: Option<u32>,
}

impl Config {
    fn get_timeout(&self) -> u64 {
        self.timeout.unwrap_or(30)
    }
    
    fn get_retries(&self) -> u32 {
        self.retries.unwrap_or(3)
    }
}
```

## The Result Enum

Result represents either success (`Ok`) or failure (`Err`).

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

### Basic Usage
```rust
use std::fs::File;
use std::io::Error;

fn main() {
    let file_result: Result<File, Error> = File::open("hello.txt");
    
    match file_result {
        Ok(file) => println!("File opened successfully"),
        Err(error) => println!("Failed to open file: {}", error),
    }
}
```

### Common Result Methods

#### `unwrap()` and `expect()`
```rust
fn divide(a: f64, b: f64) -> Result<f64, String> {
    if b == 0.0 {
        Err(String::from("Division by zero"))
    } else {
        Ok(a / b)
    }
}

fn main() {
    let result = divide(10.0, 2.0);
    println!("10 / 2 = {}", result.unwrap()); // 5
    
    let result = divide(10.0, 2.0);
    println!("10 / 2 = {}", result.expect("Division should work"));
}
```

#### `unwrap_or()` and `unwrap_or_else()`
```rust
fn main() {
    let result: Result<i32, &str> = Err("error");
    
    // Default value
    println!("Value: {}", result.unwrap_or(0));
    
    // Compute default from error
    let value = result.unwrap_or_else(|e| {
        println!("Error occurred: {}", e);
        -1
    });
}
```

#### `map()` and `map_err()`
```rust
fn main() {
    let result: Result<i32, &str> = Ok(5);
    
    // Transform success value
    let doubled = result.map(|n| n * 2);
    println!("{:?}", doubled); // Ok(10)
    
    // Transform error
    let result: Result<i32, &str> = Err("oops");
    let new_err = result.map_err(|e| format!("Error: {}", e));
    println!("{:?}", new_err); // Err("Error: oops")
}
```

### The `?` Operator

Propagate errors easily:

```rust
use std::fs::File;
use std::io::{self, Read};

fn read_username() -> Result<String, io::Error> {
    let mut file = File::open("username.txt")?;
    let mut username = String::new();
    file.read_to_string(&mut username)?;
    Ok(username)
}

// Even shorter
fn read_username_short() -> Result<String, io::Error> {
    std::fs::read_to_string("username.txt")
}
```

### Chaining with `?`
```rust
fn parse_and_double(s: &str) -> Result<i32, std::num::ParseIntError> {
    let n = s.parse::<i32>()?;
    Ok(n * 2)
}

fn process() -> Result<i32, std::num::ParseIntError> {
    let result = parse_and_double("21")?;
    Ok(result + 1)
}
```

## Converting Between Option and Result

### Option to Result
```rust
fn main() {
    let option = Some(5);
    let result: Result<i32, &str> = option.ok_or("No value");
    println!("{:?}", result); // Ok(5)
    
    let none: Option<i32> = None;
    let result = none.ok_or_else(|| "Computed error");
    println!("{:?}", result); // Err("Computed error")
}
```

### Result to Option
```rust
fn main() {
    let result: Result<i32, &str> = Ok(5);
    let option = result.ok();
    println!("{:?}", option); // Some(5)
    
    let error: Result<i32, &str> = Err("error");
    let option = error.ok();
    println!("{:?}", option); // None
}
```

## Advanced Patterns

### Collecting Results
```rust
fn main() {
    let strings = vec!["1", "2", "3"];
    let numbers: Result<Vec<i32>, _> = strings
        .iter()
        .map(|s| s.parse::<i32>())
        .collect();
    
    println!("{:?}", numbers); // Ok([1, 2, 3])
    
    let strings = vec!["1", "2", "bad"];
    let numbers: Result<Vec<i32>, _> = strings
        .iter()
        .map(|s| s.parse::<i32>())
        .collect();
    
    println!("{:?}", numbers); // Err(ParseIntError)
}
```

### Early Return Pattern
```rust
fn process_data(input: &str) -> Result<String, String> {
    // Validate input
    if input.is_empty() {
        return Err("Input is empty".to_string());
    }
    
    // Parse number
    let number: i32 = input.parse()
        .map_err(|_| "Invalid number".to_string())?;
    
    // Check range
    if number < 0 || number > 100 {
        return Err("Number out of range".to_string());
    }
    
    Ok(format!("Processed: {}", number))
}
```

### Custom Error Types
```rust
#[derive(Debug)]
enum MyError {
    IoError(std::io::Error),
    ParseError(std::num::ParseIntError),
    ValidationError(String),
}

fn complex_operation() -> Result<i32, MyError> {
    let content = std::fs::read_to_string("number.txt")
        .map_err(MyError::IoError)?;
    
    let number = content.trim().parse::<i32>()
        .map_err(MyError::ParseError)?;
    
    if number < 0 {
        return Err(MyError::ValidationError("Negative number".to_string()));
    }
    
    Ok(number * 2)
}
```

## Best Practices

1. **Use Option for nullable values**, not empty strings or magic numbers
2. **Use Result for operations that can fail**
3. **Prefer `?` operator** over explicit match when propagating errors
4. **Use descriptive error types** instead of strings when possible
5. **Consider `unwrap_or` methods** instead of match for simple defaults
6. **Document when functions return None or Err**
7. **Use `expect()` with meaningful messages** during development
8. **Avoid `unwrap()` in production code** unless you're certain it won't panic