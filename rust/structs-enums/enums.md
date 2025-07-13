# Enums in Rust

Enums allow you to define a type by enumerating its possible variants. They are one of Rust's most powerful features.

## Basic Enums

Simple enum definition:

```rust
enum Direction {
    North,
    South,
    East,
    West,
}

fn main() {
    let dir = Direction::North;
    
    match dir {
        Direction::North => println!("Going north!"),
        Direction::South => println!("Going south!"),
        Direction::East => println!("Going east!"),
        Direction::West => println!("Going west!"),
    }
}
```

## Enums with Data

Enums can hold data:

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}

fn main() {
    let msg1 = Message::Write(String::from("Hello"));
    let msg2 = Message::Move { x: 10, y: 20 };
    let msg3 = Message::ChangeColor(255, 0, 0);
    
    process_message(msg1);
}

fn process_message(msg: Message) {
    match msg {
        Message::Quit => println!("Quit"),
        Message::Move { x, y } => println!("Move to ({}, {})", x, y),
        Message::Write(text) => println!("Text: {}", text),
        Message::ChangeColor(r, g, b) => println!("Color: RGB({}, {}, {})", r, g, b),
    }
}
```

## Methods on Enums

You can define methods on enums:

```rust
impl Message {
    fn call(&self) {
        match self {
            Message::Write(text) => println!("Writing: {}", text),
            _ => println!("Other message"),
        }
    }
}

fn main() {
    let m = Message::Write(String::from("hello"));
    m.call();
}
```

## The Option Enum

Rust's way of handling null values:

```rust
fn main() {
    let some_number = Some(5);
    let some_string = Some("a string");
    let absent_number: Option<i32> = None;
    
    // Using Option
    match some_number {
        Some(n) => println!("Number: {}", n),
        None => println!("No number"),
    }
}
```

### Working with Option
```rust
fn main() {
    let x: Option<i32> = Some(5);
    let y: Option<i32> = None;
    
    // Using unwrap_or
    println!("x: {}", x.unwrap_or(0));
    println!("y: {}", y.unwrap_or(0));
    
    // Using map
    let doubled = x.map(|n| n * 2);
    println!("Doubled: {:?}", doubled);
    
    // Using and_then
    let result = x.and_then(|n| {
        if n > 0 {
            Some(n * 2)
        } else {
            None
        }
    });
    println!("Result: {:?}", result);
}
```

## The Result Enum

For error handling:

```rust
use std::fs::File;

fn main() {
    let f = File::open("hello.txt");
    
    let f = match f {
        Ok(file) => file,
        Err(error) => {
            panic!("Problem opening file: {:?}", error);
        }
    };
}
```

### Result Methods
```rust
fn divide(x: f64, y: f64) -> Result<f64, String> {
    if y == 0.0 {
        Err(String::from("Division by zero"))
    } else {
        Ok(x / y)
    }
}

fn main() {
    // Using match
    match divide(10.0, 2.0) {
        Ok(result) => println!("Result: {}", result),
        Err(e) => println!("Error: {}", e),
    }
    
    // Using unwrap_or
    let result = divide(10.0, 0.0).unwrap_or(0.0);
    println!("Result with default: {}", result);
    
    // Using ?
    fn calculate() -> Result<f64, String> {
        let x = divide(10.0, 2.0)?;
        let y = divide(x, 2.0)?;
        Ok(y)
    }
}
```

## Pattern Matching with Enums

### Destructuring
```rust
enum WebEvent {
    PageLoad,
    PageUnload,
    KeyPress(char),
    Paste(String),
    Click { x: i64, y: i64 },
}

fn inspect(event: WebEvent) {
    match event {
        WebEvent::PageLoad => println!("page loaded"),
        WebEvent::PageUnload => println!("page unloaded"),
        WebEvent::KeyPress(c) => println!("pressed '{}'", c),
        WebEvent::Paste(s) => println!("pasted \"{}\"", s),
        WebEvent::Click { x, y } => {
            println!("clicked at x={}, y={}", x, y);
        }
    }
}
```

### Guards
```rust
enum Temperature {
    Celsius(f32),
    Fahrenheit(f32),
}

fn main() {
    let temp = Temperature::Celsius(35.0);
    
    match temp {
        Temperature::Celsius(t) if t > 30.0 => println!("Hot day!"),
        Temperature::Celsius(t) => println!("Temperature: {}°C", t),
        Temperature::Fahrenheit(t) => println!("Temperature: {}°F", t),
    }
}
```

## Generic Enums

```rust
enum MyOption<T> {
    Some(T),
    None,
}

enum MyResult<T, E> {
    Ok(T),
    Err(E),
}

fn main() {
    let number: MyOption<i32> = MyOption::Some(5);
    let text: MyOption<String> = MyOption::None;
}
```

## Recursive Enums

Using `Box` for recursive types:

```rust
enum List {
    Cons(i32, Box<List>),
    Nil,
}

use List::{Cons, Nil};

fn main() {
    let list = Cons(1, Box::new(Cons(2, Box::new(Cons(3, Box::new(Nil))))));
}
```

## Practical Examples

### State Machine
```rust
#[derive(Debug)]
enum State {
    Idle,
    Running { progress: u32 },
    Completed { result: String },
    Failed { error: String },
}

struct Task {
    state: State,
}

impl Task {
    fn new() -> Self {
        Task { state: State::Idle }
    }
    
    fn start(&mut self) {
        self.state = State::Running { progress: 0 };
    }
    
    fn update_progress(&mut self, progress: u32) {
        if let State::Running { .. } = self.state {
            self.state = State::Running { progress };
        }
    }
    
    fn complete(&mut self, result: String) {
        self.state = State::Completed { result };
    }
}

fn main() {
    let mut task = Task::new();
    println!("Initial: {:?}", task.state);
    
    task.start();
    println!("Started: {:?}", task.state);
    
    task.update_progress(50);
    println!("Progress: {:?}", task.state);
    
    task.complete(String::from("Success!"));
    println!("Completed: {:?}", task.state);
}
```

### Command Pattern
```rust
enum Command {
    Create { name: String },
    Delete { id: u32 },
    Update { id: u32, name: String },
    List,
}

fn execute_command(cmd: Command) {
    match cmd {
        Command::Create { name } => {
            println!("Creating item: {}", name);
        }
        Command::Delete { id } => {
            println!("Deleting item with id: {}", id);
        }
        Command::Update { id, name } => {
            println!("Updating item {} with name: {}", id, name);
        }
        Command::List => {
            println!("Listing all items");
        }
    }
}

fn main() {
    let commands = vec![
        Command::Create { name: String::from("Task 1") },
        Command::List,
        Command::Update { id: 1, name: String::from("Updated Task") },
        Command::Delete { id: 1 },
    ];
    
    for cmd in commands {
        execute_command(cmd);
    }
}
```

### Error Types
```rust
#[derive(Debug)]
enum AppError {
    IoError(String),
    ParseError(String),
    ValidationError { field: String, message: String },
}

fn read_config() -> Result<String, AppError> {
    Err(AppError::IoError(String::from("File not found")))
}

fn main() {
    match read_config() {
        Ok(config) => println!("Config: {}", config),
        Err(e) => match e {
            AppError::IoError(msg) => eprintln!("IO Error: {}", msg),
            AppError::ParseError(msg) => eprintln!("Parse Error: {}", msg),
            AppError::ValidationError { field, message } => {
                eprintln!("Validation Error in {}: {}", field, message)
            }
        }
    }
}
```

## Best Practices

1. **Use enums for finite sets of variants**
2. **Leverage pattern matching** for exhaustive handling
3. **Use Option instead of null**
4. **Use Result for error handling**
5. **Consider using enums for state machines**
6. **Name variants clearly** and consistently
7. **Group related data in enum variants**
8. **Implement useful methods** on enums when appropriate