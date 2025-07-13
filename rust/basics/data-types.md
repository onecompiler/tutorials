# Data Types in Rust

Rust is a statically typed language, meaning it must know the types of all variables at compile time. Let's explore Rust's data types.

## Scalar Types

Scalar types represent single values. Rust has four primary scalar types:

### 1. Integer Types

Integers are whole numbers without fractional components.

| Length | Signed | Unsigned |
|--------|--------|----------|
| 8-bit  | i8     | u8       |
| 16-bit | i16    | u16      |
| 32-bit | i32    | u32      |
| 64-bit | i64    | u64      |
| 128-bit| i128   | u128     |
| arch   | isize  | usize    |

```rust
fn main() {
    let a: i32 = 42;        // Signed 32-bit integer
    let b: u64 = 100;       // Unsigned 64-bit integer
    let c: isize = -50;     // Architecture-dependent size
    
    println!("a: {}, b: {}, c: {}", a, b, c);
}
```

Number literals can use:
```rust
fn main() {
    let decimal = 98_222;      // Decimal (with separator)
    let hex = 0xff;            // Hexadecimal
    let octal = 0o77;          // Octal
    let binary = 0b1111_0000;  // Binary
    let byte = b'A';           // Byte (u8 only)
    
    println!("Decimal: {}", decimal);
    println!("Hex: {}", hex);
    println!("Octal: {}", octal);
    println!("Binary: {}", binary);
    println!("Byte: {}", byte);
}
```

### 2. Floating-Point Types

Rust has two floating-point types:

```rust
fn main() {
    let x = 2.0;      // f64 (default)
    let y: f32 = 3.0; // f32
    
    println!("x: {}, y: {}", x, y);
    
    // Floating-point operations
    let sum = x + y as f64;
    let product = x * y as f64;
    println!("Sum: {}, Product: {}", sum, product);
}
```

### 3. Boolean Type

The boolean type has two values: `true` and `false`.

```rust
fn main() {
    let is_active: bool = true;
    let is_greater = 10 > 5;
    
    println!("is_active: {}", is_active);
    println!("is_greater: {}", is_greater);
}
```

### 4. Character Type

The `char` type represents a single Unicode scalar value:

```rust
fn main() {
    let letter = 'A';
    let emoji = '😊';
    let chinese = '中';
    
    println!("Letter: {}", letter);
    println!("Emoji: {}", emoji);
    println!("Chinese: {}", chinese);
}
```

## Compound Types

Compound types can group multiple values into one type.

### 1. Tuples

Tuples group together values of different types:

```rust
fn main() {
    let tup: (i32, f64, char) = (500, 6.4, 'z');
    
    // Destructuring
    let (x, y, z) = tup;
    println!("x: {}, y: {}, z: {}", x, y, z);
    
    // Direct access
    let first = tup.0;
    let second = tup.1;
    let third = tup.2;
    println!("First: {}, Second: {}, Third: {}", first, second, third);
}
```

### 2. Arrays

Arrays have a fixed length and all elements must be the same type:

```rust
fn main() {
    let numbers = [1, 2, 3, 4, 5];
    
    // With type annotation
    let months: [&str; 12] = [
        "January", "February", "March", "April",
        "May", "June", "July", "August",
        "September", "October", "November", "December"
    ];
    
    // Initialize with same value
    let zeros = [0; 5]; // [0, 0, 0, 0, 0]
    
    // Accessing elements
    let first = numbers[0];
    let third_month = months[2];
    
    println!("First number: {}", first);
    println!("Third month: {}", third_month);
}
```

## Type Aliases

Create new names for existing types:

```rust
type Kilometers = i32;

fn main() {
    let distance: Kilometers = 100;
    println!("Distance: {} km", distance);
}
```

## Type Conversion

### Implicit vs Explicit

Rust doesn't do implicit type conversion. You must be explicit:

```rust
fn main() {
    let x: i32 = 5;
    let y: i64 = 10;
    
    // This won't compile:
    // let sum = x + y;
    
    // Explicit conversion
    let sum = x as i64 + y;
    println!("Sum: {}", sum);
}
```

### Parsing Strings

Convert strings to other types:

```rust
fn main() {
    let text = "42";
    let number: i32 = text.parse().expect("Not a valid number");
    
    // Or with turbofish syntax
    let number2 = "123".parse::<i32>().expect("Not a valid number");
    
    println!("Parsed: {} and {}", number, number2);
}
```

## Type Inference

Rust can often infer types:

```rust
fn main() {
    let x = 5;        // Inferred as i32
    let y = 2.5;      // Inferred as f64
    
    // Sometimes you need to help
    let numbers = vec![1, 2, 3]; // Vec<i32>
    let empty_vec: Vec<i32> = Vec::new(); // Type required
}
```

## Memory Size

Check the size of types:

```rust
use std::mem;

fn main() {
    println!("Size of i32: {} bytes", mem::size_of::<i32>());
    println!("Size of f64: {} bytes", mem::size_of::<f64>());
    println!("Size of char: {} bytes", mem::size_of::<char>());
    println!("Size of bool: {} bytes", mem::size_of::<bool>());
    println!("Size of (i32, i32): {} bytes", mem::size_of::<(i32, i32)>());
}
```

## Practical Examples

### Working with Different Types
```rust
fn main() {
    // Personal data
    let name = "Alice";
    let age: u8 = 30;
    let height: f32 = 5.6;
    let is_student = false;
    let grade = 'A';
    
    println!("{} is {} years old", name, age);
    println!("Height: {} feet", height);
    println!("Student: {}, Grade: {}", is_student, grade);
}
```

### Type Safety Example
```rust
fn main() {
    let mut score = 0;
    
    // This is safe
    score = score + 10;
    
    // This won't compile (type safety)
    // score = score + 0.5; // Error: mismatched types
    
    // Must be explicit about conversion
    let final_score = score as f64 + 0.5;
    println!("Final score: {}", final_score);
}
```