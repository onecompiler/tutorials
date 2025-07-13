# Operators in Rust

Rust provides a comprehensive set of operators for performing various operations on values. Let's explore each category.

## Arithmetic Operators

Basic mathematical operations:

```rust
fn main() {
    let a = 10;
    let b = 3;
    
    println!("Addition: {} + {} = {}", a, b, a + b);
    println!("Subtraction: {} - {} = {}", a, b, a - b);
    println!("Multiplication: {} * {} = {}", a, b, a * b);
    println!("Division: {} / {} = {}", a, b, a / b);
    println!("Remainder: {} % {} = {}", a, b, a % b);
}
```

### Integer Division vs Float Division
```rust
fn main() {
    // Integer division truncates
    let int_division = 7 / 2;
    println!("7 / 2 = {}", int_division); // 3
    
    // Float division
    let float_division = 7.0 / 2.0;
    println!("7.0 / 2.0 = {}", float_division); // 3.5
}
```

## Comparison Operators

Used to compare values:

```rust
fn main() {
    let x = 5;
    let y = 10;
    
    println!("{} == {} is {}", x, y, x == y); // Equal to
    println!("{} != {} is {}", x, y, x != y); // Not equal to
    println!("{} < {} is {}", x, y, x < y);   // Less than
    println!("{} > {} is {}", x, y, x > y);   // Greater than
    println!("{} <= {} is {}", x, y, x <= y); // Less than or equal
    println!("{} >= {} is {}", x, y, x >= y); // Greater than or equal
}
```

## Logical Operators

Used with boolean values:

```rust
fn main() {
    let sunny = true;
    let warm = false;
    
    println!("sunny AND warm: {}", sunny && warm);     // Logical AND
    println!("sunny OR warm: {}", sunny || warm);      // Logical OR
    println!("NOT sunny: {}", !sunny);                 // Logical NOT
    
    // Short-circuit evaluation
    let result = false && panic!("This won't execute");
    println!("Short-circuit worked: {}", result);
}
```

## Bitwise Operators

Operate on individual bits:

```rust
fn main() {
    let a: u8 = 0b1010; // 10 in binary
    let b: u8 = 0b1100; // 12 in binary
    
    println!("a = {:08b} ({})", a, a);
    println!("b = {:08b} ({})", b, b);
    println!("AND: {:08b} ({})", a & b, a & b);
    println!("OR:  {:08b} ({})", a | b, a | b);
    println!("XOR: {:08b} ({})", a ^ b, a ^ b);
    println!("NOT a: {:08b} ({})", !a, !a);
    
    // Bit shifts
    println!("a << 1: {:08b} ({})", a << 1, a << 1);
    println!("a >> 1: {:08b} ({})", a >> 1, a >> 1);
}
```

## Assignment Operators

Basic and compound assignment:

```rust
fn main() {
    let mut x = 10;
    println!("Initial x: {}", x);
    
    x += 5;  // Same as x = x + 5
    println!("After x += 5: {}", x);
    
    x -= 3;  // Same as x = x - 3
    println!("After x -= 3: {}", x);
    
    x *= 2;  // Same as x = x * 2
    println!("After x *= 2: {}", x);
    
    x /= 4;  // Same as x = x / 4
    println!("After x /= 4: {}", x);
    
    x %= 5;  // Same as x = x % 5
    println!("After x %= 5: {}", x);
}
```

## Range Operators

Create ranges of values:

```rust
fn main() {
    // Exclusive range (doesn't include end)
    for i in 1..5 {
        print!("{} ", i); // Prints: 1 2 3 4
    }
    println!();
    
    // Inclusive range
    for i in 1..=5 {
        print!("{} ", i); // Prints: 1 2 3 4 5
    }
    println!();
    
    // Range from start
    let arr = [1, 2, 3, 4, 5];
    let slice = &arr[2..]; // [3, 4, 5]
    println!("Slice from 2: {:?}", slice);
    
    // Range to end
    let slice = &arr[..3]; // [1, 2, 3]
    println!("Slice to 3: {:?}", slice);
}
```

## Type Cast Operator

Convert between types using `as`:

```rust
fn main() {
    let x: i32 = 42;
    let y: f64 = x as f64;
    
    println!("i32 value: {}", x);
    println!("as f64: {}", y);
    
    // Truncation when casting floats to integers
    let pi = 3.14159;
    let truncated = pi as i32;
    println!("PI truncated: {}", truncated); // 3
    
    // Character to number
    let letter = 'A';
    let ascii = letter as u8;
    println!("'A' as u8: {}", ascii); // 65
}
```

## Operator Precedence

Operators follow standard mathematical precedence:

```rust
fn main() {
    // Multiplication before addition
    let result1 = 2 + 3 * 4;
    println!("2 + 3 * 4 = {}", result1); // 14
    
    // Use parentheses to change order
    let result2 = (2 + 3) * 4;
    println!("(2 + 3) * 4 = {}", result2); // 20
    
    // Complex expression
    let x = 10;
    let y = 3;
    let complex = x + y * 2 - x / y;
    println!("10 + 3 * 2 - 10 / 3 = {}", complex);
}
```

## Pattern Matching Operator

The match operator for pattern matching:

```rust
fn main() {
    let number = 3;
    
    match number {
        1 => println!("One"),
        2 => println!("Two"),
        3 => println!("Three"),
        _ => println!("Something else"),
    }
    
    // Match with ranges
    let score = 85;
    let grade = match score {
        90..=100 => 'A',
        80..=89 => 'B',
        70..=79 => 'C',
        60..=69 => 'D',
        _ => 'F',
    };
    println!("Score {} = Grade {}", score, grade);
}
```

## Practical Examples

### Calculator Function
```rust
fn calculate(a: f64, b: f64, op: char) -> f64 {
    match op {
        '+' => a + b,
        '-' => a - b,
        '*' => a * b,
        '/' => a / b,
        '%' => a % b,
        _ => {
            println!("Unknown operator");
            0.0
        }
    }
}

fn main() {
    println!("10 + 5 = {}", calculate(10.0, 5.0, '+'));
    println!("10 - 5 = {}", calculate(10.0, 5.0, '-'));
    println!("10 * 5 = {}", calculate(10.0, 5.0, '*'));
    println!("10 / 5 = {}", calculate(10.0, 5.0, '/'));
}
```

### Bit Flags
```rust
fn main() {
    const READ: u8 = 0b100;
    const WRITE: u8 = 0b010;
    const EXECUTE: u8 = 0b001;
    
    let permissions = READ | WRITE; // Combine permissions
    
    println!("Has read: {}", permissions & READ != 0);
    println!("Has write: {}", permissions & WRITE != 0);
    println!("Has execute: {}", permissions & EXECUTE != 0);
}
```