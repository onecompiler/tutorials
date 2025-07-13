# Functions in Rust

Functions are fundamental building blocks in Rust. They allow you to organize code into reusable units.

## Function Basics

Define a function using the `fn` keyword:

```rust
fn main() {
    greet();
}

fn greet() {
    println!("Hello, Rust!");
}
```

## Function Parameters

Functions can accept parameters:

```rust
fn main() {
    greet_user("Alice");
    greet_user("Bob");
}

fn greet_user(name: &str) {
    println!("Hello, {}!", name);
}
```

### Multiple Parameters
```rust
fn main() {
    print_sum(5, 10);
}

fn print_sum(x: i32, y: i32) {
    println!("{} + {} = {}", x, y, x + y);
}
```

## Return Values

Use `->` to specify return type:

```rust
fn main() {
    let result = add(5, 3);
    println!("5 + 3 = {}", result);
}

fn add(x: i32, y: i32) -> i32 {
    x + y  // No semicolon - this is the return value
}
```

### Explicit Return
```rust
fn main() {
    let result = calculate(10, 5, '+');
    println!("Result: {}", result);
}

fn calculate(a: i32, b: i32, op: char) -> i32 {
    if op == '+' {
        return a + b;
    } else if op == '-' {
        return a - b;
    }
    
    0  // Default return
}
```

## Statements vs Expressions

Understanding the difference is crucial in Rust:

```rust
fn main() {
    let y = {
        let x = 3;
        x + 1  // Expression (no semicolon)
    };
    
    println!("y = {}", y);  // Prints: y = 4
}
```

### Functions are Expressions
```rust
fn main() {
    let result = add_one(5);
    println!("Result: {}", result);
}

fn add_one(x: i32) -> i32 {
    x + 1  // The entire function body is an expression
}
```

## Function Pointers

Functions can be stored in variables:

```rust
fn main() {
    let my_func: fn(i32, i32) -> i32 = add;
    let result = my_func(5, 3);
    println!("Result: {}", result);
}

fn add(a: i32, b: i32) -> i32 {
    a + b
}
```

## Generic Functions

Functions can work with multiple types:

```rust
fn main() {
    let int_result = largest(5, 10);
    let char_result = largest('a', 'z');
    
    println!("Larger int: {}", int_result);
    println!("Larger char: {}", char_result);
}

fn largest<T: PartialOrd>(a: T, b: T) -> T {
    if a > b {
        a
    } else {
        b
    }
}
```

## Methods

Methods are functions defined within the context of a struct:

```rust
struct Rectangle {
    width: f64,
    height: f64,
}

impl Rectangle {
    // Method
    fn area(&self) -> f64 {
        self.width * self.height
    }
    
    // Associated function (like static method)
    fn new(width: f64, height: f64) -> Rectangle {
        Rectangle { width, height }
    }
}

fn main() {
    let rect = Rectangle::new(10.0, 20.0);
    println!("Area: {}", rect.area());
}
```

## Higher-Order Functions

Functions that take or return other functions:

```rust
fn main() {
    let numbers = vec![1, 2, 3, 4, 5];
    let result = apply_to_vec(&numbers, double);
    println!("Doubled: {:?}", result);
}

fn apply_to_vec(vec: &Vec<i32>, f: fn(i32) -> i32) -> Vec<i32> {
    vec.iter().map(|&x| f(x)).collect()
}

fn double(x: i32) -> i32 {
    x * 2
}
```

## Recursion

Functions can call themselves:

```rust
fn main() {
    println!("Factorial of 5: {}", factorial(5));
    println!("Fibonacci 10th: {}", fibonacci(10));
}

fn factorial(n: u32) -> u32 {
    if n <= 1 {
        1
    } else {
        n * factorial(n - 1)
    }
}

fn fibonacci(n: u32) -> u32 {
    match n {
        0 => 0,
        1 => 1,
        _ => fibonacci(n - 1) + fibonacci(n - 2),
    }
}
```

## Function Overloading

Rust doesn't support traditional overloading, but you can use traits:

```rust
trait Display {
    fn display(&self);
}

impl Display for i32 {
    fn display(&self) {
        println!("Integer: {}", self);
    }
}

impl Display for String {
    fn display(&self) {
        println!("String: {}", self);
    }
}

fn main() {
    42.display();
    String::from("Hello").display();
}
```

## Diverging Functions

Functions that never return use `!` type:

```rust
fn main() {
    let x = 5;
    if x < 0 {
        crash("x cannot be negative");
    }
    println!("x is positive");
}

fn crash(msg: &str) -> ! {
    panic!("{}", msg);
}
```

## Best Practices

### 1. Keep Functions Small
```rust
// Good
fn calculate_area(width: f64, height: f64) -> f64 {
    width * height
}

fn calculate_perimeter(width: f64, height: f64) -> f64 {
    2.0 * (width + height)
}

// Less good
fn calculate_rectangle_properties(width: f64, height: f64) -> (f64, f64) {
    let area = width * height;
    let perimeter = 2.0 * (width + height);
    (area, perimeter)
}
```

### 2. Use Descriptive Names
```rust
// Good
fn is_valid_email(email: &str) -> bool {
    email.contains('@') && email.contains('.')
}

// Less good
fn check(s: &str) -> bool {
    s.contains('@') && s.contains('.')
}
```

### 3. Document Your Functions
```rust
/// Calculates the nth Fibonacci number
/// 
/// # Arguments
/// * `n` - The position in the Fibonacci sequence
/// 
/// # Returns
/// The nth Fibonacci number
fn fibonacci(n: u32) -> u32 {
    match n {
        0 => 0,
        1 => 1,
        _ => fibonacci(n - 1) + fibonacci(n - 2),
    }
}
```

## Practical Examples

### Password Validator
```rust
fn main() {
    let password = "MyP@ssw0rd";
    if is_strong_password(password) {
        println!("Password is strong");
    } else {
        println!("Password is weak");
    }
}

fn is_strong_password(password: &str) -> bool {
    password.len() >= 8 &&
    password.chars().any(|c| c.is_uppercase()) &&
    password.chars().any(|c| c.is_lowercase()) &&
    password.chars().any(|c| c.is_numeric()) &&
    password.chars().any(|c| !c.is_alphanumeric())
}
```

### Temperature Converter
```rust
fn main() {
    let celsius = 25.0;
    let fahrenheit = celsius_to_fahrenheit(celsius);
    println!("{}°C = {}°F", celsius, fahrenheit);
    
    let fahrenheit = 77.0;
    let celsius = fahrenheit_to_celsius(fahrenheit);
    println!("{}°F = {}°C", fahrenheit, celsius);
}

fn celsius_to_fahrenheit(c: f64) -> f64 {
    c * 9.0 / 5.0 + 32.0
}

fn fahrenheit_to_celsius(f: f64) -> f64 {
    (f - 32.0) * 5.0 / 9.0
}
```