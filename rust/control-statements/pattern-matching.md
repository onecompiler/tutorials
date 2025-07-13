# Pattern Matching in Rust

Pattern matching is one of Rust's most powerful features, allowing you to compare values against patterns and execute code based on which pattern matches.

## The `match` Expression

Basic syntax of `match`:

```rust
fn main() {
    let number = 13;
    
    match number {
        1 => println!("One"),
        2 => println!("Two"),
        3..=12 => println!("Between 3 and 12"),
        13 => println!("Thirteen"),
        _ => println!("Something else"),
    }
}
```

## Matching Multiple Values

```rust
fn main() {
    let day = 5;
    
    let day_type = match day {
        1 | 2 | 3 | 4 | 5 => "Weekday",
        6 | 7 => "Weekend",
        _ => "Invalid day",
    };
    
    println!("Day {} is a {}", day, day_type);
}
```

## Destructuring in Patterns

### Tuples
```rust
fn main() {
    let point = (3, 5);
    
    match point {
        (0, 0) => println!("Origin"),
        (0, y) => println!("On Y axis at {}", y),
        (x, 0) => println!("On X axis at {}", x),
        (x, y) => println!("At ({}, {})", x, y),
    }
}
```

### Structs
```rust
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let point = Point { x: 0, y: 7 };
    
    match point {
        Point { x: 0, y: 0 } => println!("Origin"),
        Point { x: 0, y } => println!("On Y axis at {}", y),
        Point { x, y: 0 } => println!("On X axis at {}", x),
        Point { x, y } => println!("At ({}, {})", x, y),
    }
}
```

### Enums
```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}

fn main() {
    let msg = Message::Move { x: 10, y: 20 };
    
    match msg {
        Message::Quit => println!("Quit"),
        Message::Move { x, y } => println!("Move to ({}, {})", x, y),
        Message::Write(text) => println!("Text: {}", text),
        Message::ChangeColor(r, g, b) => println!("RGB: ({}, {}, {})", r, g, b),
    }
}
```

## Guards in Patterns

Add extra conditions with guards:

```rust
fn main() {
    let num = Some(4);
    
    match num {
        Some(x) if x < 5 => println!("Less than five: {}", x),
        Some(x) if x == 5 => println!("Equal to five"),
        Some(x) => println!("Greater than five: {}", x),
        None => println!("No value"),
    }
}
```

## Binding with `@`

Bind matched values to variables:

```rust
fn main() {
    let age = 15;
    
    match age {
        0 => println!("Baby"),
        n @ 1..=12 => println!("Child of age {}", n),
        n @ 13..=19 => println!("Teenager of age {}", n),
        n => println!("Adult of age {}", n),
    }
}
```

## Ignoring Values

### Ignore Entire Value with `_`
```rust
fn main() {
    let numbers = (2, 4, 8, 16, 32);
    
    match numbers {
        (first, _, third, _, fifth) => {
            println!("Some numbers: {}, {}, {}", first, third, fifth)
        }
    }
}
```

### Ignore Remaining Parts with `..`
```rust
fn main() {
    let numbers = (2, 4, 8, 16, 32);
    
    match numbers {
        (first, .., last) => {
            println!("First: {}, Last: {}", first, last)
        }
    }
}
```

## Reference Patterns

Match on references:

```rust
fn main() {
    let reference = &4;
    
    match reference {
        &val => println!("Got a value: {}", val),
    }
    
    // Or using ref
    let value = 5;
    match value {
        ref r => println!("Got a reference to: {}", r),
    }
}
```

## Matching Option<T>

Common pattern with `Option`:

```rust
fn divide(dividend: f64, divisor: f64) -> Option<f64> {
    if divisor == 0.0 {
        None
    } else {
        Some(dividend / divisor)
    }
}

fn main() {
    let result = divide(10.0, 2.0);
    
    match result {
        Some(x) => println!("Result: {}", x),
        None => println!("Cannot divide by zero"),
    }
}
```

## Matching Result<T, E>

Handle errors elegantly:

```rust
use std::fs::File;
use std::io::ErrorKind;

fn main() {
    let file_result = File::open("hello.txt");
    
    let file = match file_result {
        Ok(file) => file,
        Err(error) => match error.kind() {
            ErrorKind::NotFound => {
                println!("File not found");
                return;
            }
            other_error => {
                panic!("Problem opening file: {:?}", other_error);
            }
        },
    };
}
```

## Practical Examples

### State Machine
```rust
enum State {
    Waiting,
    Processing,
    Finished,
}

fn process_state(state: State) -> State {
    match state {
        State::Waiting => {
            println!("Starting to process...");
            State::Processing
        }
        State::Processing => {
            println!("Processing complete!");
            State::Finished
        }
        State::Finished => {
            println!("Already finished");
            State::Finished
        }
    }
}

fn main() {
    let mut state = State::Waiting;
    
    for _ in 0..3 {
        state = process_state(state);
    }
}
```

### Command Parser
```rust
enum Command {
    Up(i32),
    Down(i32),
    Left(i32),
    Right(i32),
    Quit,
}

fn execute_command(cmd: Command) {
    match cmd {
        Command::Up(distance) => println!("Moving up {} units", distance),
        Command::Down(distance) => println!("Moving down {} units", distance),
        Command::Left(distance) => println!("Moving left {} units", distance),
        Command::Right(distance) => println!("Moving right {} units", distance),
        Command::Quit => println!("Quitting..."),
    }
}

fn main() {
    let commands = vec![
        Command::Up(5),
        Command::Right(3),
        Command::Down(2),
        Command::Quit,
    ];
    
    for cmd in commands {
        execute_command(cmd);
    }
}
```

### Exhaustive Matching
```rust
enum Color {
    Red,
    Green,
    Blue,
}

fn main() {
    let color = Color::Green;
    
    // This match is exhaustive - covers all variants
    let hex = match color {
        Color::Red => "#FF0000",
        Color::Green => "#00FF00",
        Color::Blue => "#0000FF",
    };
    
    println!("Hex color: {}", hex);
}
```

## Best Practices

1. Always handle all cases (exhaustive matching)
2. Put more specific patterns before general ones
3. Use `_` for catch-all cases
4. Consider using `if let` for single pattern matches
5. Use guards for additional conditions
6. Leverage destructuring to extract values