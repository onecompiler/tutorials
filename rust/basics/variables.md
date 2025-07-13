# Variables and Mutability in Rust

Rust's approach to variables is unique - variables are immutable by default. This design choice helps prevent bugs and makes code easier to reason about.

## Immutable Variables

By default, variables in Rust are immutable:

```rust
fn main() {
    let x = 5;
    println!("The value of x is: {}", x);
    
    // This will cause a compile error:
    // x = 6; // Error: cannot assign twice to immutable variable
}
```

## Mutable Variables

To make a variable mutable, use the `mut` keyword:

```rust
fn main() {
    let mut x = 5;
    println!("The value of x is: {}", x);
    
    x = 6; // This is allowed
    println!("The value of x is now: {}", x);
}
```

## Constants

Constants are always immutable and must have a type annotation:

```rust
const MAX_POINTS: u32 = 100_000;
const PI: f64 = 3.14159;

fn main() {
    println!("Max points: {}", MAX_POINTS);
    println!("Pi value: {}", PI);
}
```

Key differences between constants and immutable variables:
- Constants use `const` keyword, not `let`
- Type annotation is required for constants
- Constants can be declared in any scope
- Constants must be set to a constant expression

## Shadowing

Rust allows you to declare a new variable with the same name as a previous variable:

```rust
fn main() {
    let x = 5;
    let x = x + 1; // Shadows the previous x
    
    {
        let x = x * 2; // Shadows in inner scope
        println!("Inner scope x: {}", x); // Prints: 12
    }
    
    println!("Outer scope x: {}", x); // Prints: 6
}
```

Shadowing allows changing the type:
```rust
fn main() {
    let spaces = "   ";
    let spaces = spaces.len(); // Now it's a number
    println!("Number of spaces: {}", spaces);
}
```

## Variable Naming Conventions

Rust follows these naming conventions:
- Variables: `snake_case`
- Constants: `SCREAMING_SNAKE_CASE`
- Types: `CamelCase`

```rust
const MAX_SIZE: usize = 100;

fn main() {
    let user_name = "Alice";
    let age_in_years = 30;
    let is_active = true;
}
```

## Type Annotations

Rust can usually infer types, but sometimes you need to specify:

```rust
fn main() {
    // Type inference
    let x = 5; // Rust infers i32
    
    // Explicit type annotation
    let y: i64 = 5;
    
    // Parsing requires type annotation
    let guess: u32 = "42".parse().expect("Not a number!");
}
```

## Multiple Variable Declaration

You can declare multiple variables at once using patterns:

```rust
fn main() {
    // Tuple destructuring
    let (x, y, z) = (1, 2, 3);
    
    // With type annotations
    let (a, b): (i32, f64) = (10, 3.14);
    
    println!("x={}, y={}, z={}", x, y, z);
    println!("a={}, b={}", a, b);
}
```

## Unused Variables

Rust warns about unused variables. Prefix with underscore to suppress warning:

```rust
fn main() {
    let _unused_variable = 42; // No warning
    let used_variable = 10;
    println!("Used: {}", used_variable);
}
```

## Best Practices

1. **Prefer immutability**: Only use `mut` when necessary
2. **Use descriptive names**: `user_count` instead of `uc`
3. **Use constants for magic numbers**: Define constants instead of hardcoding values
4. **Shadow wisely**: Use shadowing for transformations, not confusion

## Examples

### Temperature Converter
```rust
fn main() {
    let celsius = 25.0;
    let fahrenheit = celsius * 9.0/5.0 + 32.0;
    
    println!("{}°C = {}°F", celsius, fahrenheit);
}
```

### Using Mutability
```rust
fn main() {
    let mut counter = 0;
    
    println!("Initial counter: {}", counter);
    
    counter += 1;
    println!("After increment: {}", counter);
    
    counter *= 2;
    println!("After doubling: {}", counter);
}
```

### Constants in Practice
```rust
const SPEED_OF_LIGHT: u64 = 299_792_458; // m/s
const SECONDS_IN_HOUR: u32 = 3_600;

fn main() {
    let hours = 2;
    let distance = SPEED_OF_LIGHT * (hours * SECONDS_IN_HOUR) as u64;
    
    println!("Light travels {} meters in {} hours", distance, hours);
}
```