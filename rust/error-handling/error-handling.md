# Error Handling in Rust

Rust's approach to error handling is explicit and robust, using `Result<T, E>` and `Option<T>` types instead of exceptions.

## Types of Errors

Rust has two main categories of errors:

1. **Recoverable errors** - Use `Result<T, E>`
2. **Unrecoverable errors** - Use `panic!`

## The Result Type

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

### Basic Result Usage
```rust
use std::fs::File;

fn main() {
    let file = File::open("hello.txt");
    
    match file {
        Ok(file) => println!("File opened successfully"),
        Err(error) => println!("Failed to open file: {}", error),
    }
}
```

## Propagating Errors

### Using Match
```rust
use std::fs::File;
use std::io::{self, Read};

fn read_username_from_file() -> Result<String, io::Error> {
    let file = File::open("username.txt");
    
    let mut file = match file {
        Ok(file) => file,
        Err(e) => return Err(e),
    };
    
    let mut username = String::new();
    
    match file.read_to_string(&mut username) {
        Ok(_) => Ok(username),
        Err(e) => Err(e),
    }
}
```

### Using the ? Operator
```rust
use std::fs::File;
use std::io::{self, Read};

fn read_username_from_file() -> Result<String, io::Error> {
    let mut file = File::open("username.txt")?;
    let mut username = String::new();
    file.read_to_string(&mut username)?;
    Ok(username)
}

// Even shorter
fn read_username_from_file_short() -> Result<String, io::Error> {
    std::fs::read_to_string("username.txt")
}
```

### Chaining with ?
```rust
use std::fs::File;
use std::io::{self, Read};

fn read_username_from_file() -> Result<String, io::Error> {
    let mut username = String::new();
    File::open("username.txt")?.read_to_string(&mut username)?;
    Ok(username)
}
```

## Custom Error Types

### Simple Error Enum
```rust
#[derive(Debug)]
enum MathError {
    DivisionByZero,
    NegativeSquareRoot,
}

fn divide(a: f64, b: f64) -> Result<f64, MathError> {
    if b == 0.0 {
        Err(MathError::DivisionByZero)
    } else {
        Ok(a / b)
    }
}

fn sqrt(x: f64) -> Result<f64, MathError> {
    if x < 0.0 {
        Err(MathError::NegativeSquareRoot)
    } else {
        Ok(x.sqrt())
    }
}
```

### Implementing Error Trait
```rust
use std::fmt;
use std::error::Error;

#[derive(Debug)]
struct AppError {
    message: String,
    code: u32,
}

impl fmt::Display for AppError {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "Error {}: {}", self.code, self.message)
    }
}

impl Error for AppError {}

fn do_something() -> Result<(), AppError> {
    Err(AppError {
        message: String::from("Something went wrong"),
        code: 500,
    })
}
```

## Error Conversion

### From Trait
```rust
use std::fs::File;
use std::io;
use std::num::ParseIntError;

#[derive(Debug)]
enum AppError {
    Io(io::Error),
    Parse(ParseIntError),
}

impl From<io::Error> for AppError {
    fn from(error: io::Error) -> Self {
        AppError::Io(error)
    }
}

impl From<ParseIntError> for AppError {
    fn from(error: ParseIntError) -> Self {
        AppError::Parse(error)
    }
}

fn read_number() -> Result<i32, AppError> {
    let contents = std::fs::read_to_string("number.txt")?;
    let number = contents.trim().parse()?;
    Ok(number)
}
```

## Result Methods

### Combinators
```rust
fn main() {
    let good_result: Result<i32, &str> = Ok(10);
    let bad_result: Result<i32, &str> = Err("error");
    
    // map - transform Ok value
    let doubled = good_result.map(|x| x * 2);
    println!("{:?}", doubled); // Ok(20)
    
    // map_err - transform Err value
    let new_err = bad_result.map_err(|e| format!("Got error: {}", e));
    println!("{:?}", new_err);
    
    // and_then - chain operations that return Result
    let result = good_result.and_then(|x| {
        if x > 5 {
            Ok(x * 2)
        } else {
            Err("too small")
        }
    });
}
```

### Extracting Values
```rust
fn main() {
    let result: Result<i32, &str> = Ok(42);
    
    // unwrap_or - provide default
    let value = result.unwrap_or(0);
    
    // unwrap_or_else - compute default
    let value = result.unwrap_or_else(|e| {
        println!("Error: {}", e);
        0
    });
    
    // ok() - convert to Option
    let option = result.ok();
    
    // is_ok() and is_err()
    if result.is_ok() {
        println!("Success!");
    }
}
```

## Main Function Error Handling

```rust
use std::error::Error;
use std::fs::File;

fn main() -> Result<(), Box<dyn Error>> {
    let file = File::open("hello.txt")?;
    
    // More operations...
    
    Ok(())
}
```

## Error Context

### Adding Context
```rust
use std::fs::File;
use std::io;

fn read_config() -> Result<String, String> {
    std::fs::read_to_string("config.txt")
        .map_err(|e| format!("Failed to read config: {}", e))
}

// Using a library like anyhow
// use anyhow::{Context, Result};
// 
// fn read_config() -> Result<String> {
//     std::fs::read_to_string("config.txt")
//         .context("Failed to read configuration file")
// }
```

## Practical Examples

### File Operations with Error Handling
```rust
use std::fs::{self, File};
use std::io::{self, Write};
use std::path::Path;

#[derive(Debug)]
enum FileError {
    IoError(io::Error),
    AlreadyExists,
}

fn create_file_safe(path: &str, content: &str) -> Result<(), FileError> {
    if Path::new(path).exists() {
        return Err(FileError::AlreadyExists);
    }
    
    let mut file = File::create(path)
        .map_err(FileError::IoError)?;
    
    file.write_all(content.as_bytes())
        .map_err(FileError::IoError)?;
    
    Ok(())
}

fn main() {
    match create_file_safe("test.txt", "Hello, World!") {
        Ok(()) => println!("File created successfully"),
        Err(FileError::AlreadyExists) => println!("File already exists"),
        Err(FileError::IoError(e)) => println!("IO error: {}", e),
    }
}
```

### Configuration Parser
```rust
use std::collections::HashMap;

#[derive(Debug)]
enum ConfigError {
    MissingKey(String),
    InvalidValue(String),
}

struct Config {
    values: HashMap<String, String>,
}

impl Config {
    fn get(&self, key: &str) -> Result<&String, ConfigError> {
        self.values.get(key)
            .ok_or_else(|| ConfigError::MissingKey(key.to_string()))
    }
    
    fn get_int(&self, key: &str) -> Result<i32, ConfigError> {
        let value = self.get(key)?;
        value.parse()
            .map_err(|_| ConfigError::InvalidValue(format!("{} is not a valid integer", value)))
    }
}
```

### Validation Chain
```rust
#[derive(Debug)]
struct User {
    username: String,
    email: String,
    age: u8,
}

#[derive(Debug)]
enum ValidationError {
    EmptyUsername,
    InvalidEmail,
    InvalidAge,
}

fn validate_username(username: &str) -> Result<(), ValidationError> {
    if username.is_empty() {
        Err(ValidationError::EmptyUsername)
    } else {
        Ok(())
    }
}

fn validate_email(email: &str) -> Result<(), ValidationError> {
    if email.contains('@') {
        Ok(())
    } else {
        Err(ValidationError::InvalidEmail)
    }
}

fn validate_age(age: u8) -> Result<(), ValidationError> {
    if age >= 18 {
        Ok(())
    } else {
        Err(ValidationError::InvalidAge)
    }
}

fn create_user(username: String, email: String, age: u8) -> Result<User, ValidationError> {
    validate_username(&username)?;
    validate_email(&email)?;
    validate_age(age)?;
    
    Ok(User { username, email, age })
}
```

## Best Practices

1. **Use Result for recoverable errors**, panic! for bugs
2. **Propagate errors with ?** instead of unwrapping in libraries
3. **Create custom error types** for domain-specific errors
4. **Provide context** when converting errors
5. **Use `expect` with descriptive messages** during development
6. **Avoid `unwrap()` in production code**
7. **Consider using error handling libraries** like `anyhow` or `thiserror`
8. **Document error conditions** in function documentation