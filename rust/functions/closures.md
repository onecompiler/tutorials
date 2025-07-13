# Closures in Rust

Closures are anonymous functions that can capture values from their surrounding scope. They're similar to lambdas in other languages.

## Basic Closure Syntax

```rust
fn main() {
    // Basic closure
    let add_one = |x| x + 1;
    
    let result = add_one(5);
    println!("Result: {}", result); // 6
}
```

## Type Inference

Closures can infer parameter and return types:

```rust
fn main() {
    // Type inference
    let add = |x, y| x + y;
    
    let sum = add(5, 3);
    println!("Sum: {}", sum); // 8
    
    // Explicit types
    let multiply = |x: i32, y: i32| -> i32 { x * y };
    println!("Product: {}", multiply(4, 5)); // 20
}
```

## Capturing Variables

Closures can capture variables from their environment:

```rust
fn main() {
    let multiplier = 3;
    
    let multiply_by_three = |x| x * multiplier;
    
    println!("5 × 3 = {}", multiply_by_three(5)); // 15
}
```

## Capture Modes

Closures capture variables in three ways:

### 1. By Immutable Reference
```rust
fn main() {
    let message = String::from("Hello");
    
    let print_message = || println!("{}", message);
    
    print_message();
    print_message();
    
    // message is still accessible
    println!("Original: {}", message);
}
```

### 2. By Mutable Reference
```rust
fn main() {
    let mut count = 0;
    
    let mut increment = || {
        count += 1;
        println!("Count: {}", count);
    };
    
    increment(); // Count: 1
    increment(); // Count: 2
}
```

### 3. By Value (Move)
```rust
fn main() {
    let name = String::from("Alice");
    
    let consume_name = move || {
        println!("Name: {}", name);
    };
    
    consume_name();
    // name is no longer accessible here
    // println!("{}", name); // Error!
}
```

## Function Traits

Closures implement one or more of these traits:

### `Fn` - Immutable Borrow
```rust
fn apply_twice<F>(f: F, x: i32) -> i32
where
    F: Fn(i32) -> i32,
{
    f(f(x))
}

fn main() {
    let double = |x| x * 2;
    let result = apply_twice(double, 5);
    println!("Result: {}", result); // 20
}
```

### `FnMut` - Mutable Borrow
```rust
fn apply_to_vec<F>(vec: &mut Vec<i32>, mut f: F)
where
    F: FnMut(&mut i32),
{
    for item in vec {
        f(item);
    }
}

fn main() {
    let mut numbers = vec![1, 2, 3];
    apply_to_vec(&mut numbers, |x| *x *= 2);
    println!("Doubled: {:?}", numbers); // [2, 4, 6]
}
```

### `FnOnce` - Takes Ownership
```rust
fn consume_with<F>(f: F)
where
    F: FnOnce() -> String,
{
    println!("Result: {}", f());
}

fn main() {
    let msg = String::from("Hello");
    consume_with(move || msg);
    // msg is no longer available
}
```

## Closures as Parameters

```rust
fn main() {
    let numbers = vec![1, 2, 3, 4, 5];
    
    // Using closures with iterator methods
    let squares: Vec<i32> = numbers.iter()
        .map(|&x| x * x)
        .collect();
    println!("Squares: {:?}", squares);
    
    let evens: Vec<&i32> = numbers.iter()
        .filter(|&&x| x % 2 == 0)
        .collect();
    println!("Evens: {:?}", evens);
}
```

## Returning Closures

```rust
fn make_adder(x: i32) -> impl Fn(i32) -> i32 {
    move |y| x + y
}

fn main() {
    let add_5 = make_adder(5);
    println!("10 + 5 = {}", add_5(10)); // 15
    
    let add_10 = make_adder(10);
    println!("20 + 10 = {}", add_10(20)); // 30
}
```

## Complex Examples

### Closure Factory
```rust
fn create_calculator(op: char) -> Box<dyn Fn(i32, i32) -> i32> {
    match op {
        '+' => Box::new(|a, b| a + b),
        '-' => Box::new(|a, b| a - b),
        '*' => Box::new(|a, b| a * b),
        '/' => Box::new(|a, b| a / b),
        _ => Box::new(|_, _| 0),
    }
}

fn main() {
    let add = create_calculator('+');
    let multiply = create_calculator('*');
    
    println!("5 + 3 = {}", add(5, 3));
    println!("5 * 3 = {}", multiply(5, 3));
}
```

### Memoization with Closures
```rust
use std::collections::HashMap;

struct Cacher<T>
where
    T: Fn(u32) -> u32,
{
    calculation: T,
    values: HashMap<u32, u32>,
}

impl<T> Cacher<T>
where
    T: Fn(u32) -> u32,
{
    fn new(calculation: T) -> Cacher<T> {
        Cacher {
            calculation,
            values: HashMap::new(),
        }
    }
    
    fn value(&mut self, arg: u32) -> u32 {
        match self.values.get(&arg) {
            Some(&v) => v,
            None => {
                let v = (self.calculation)(arg);
                self.values.insert(arg, v);
                v
            }
        }
    }
}

fn main() {
    let mut expensive_closure = Cacher::new(|num| {
        println!("Calculating slowly...");
        std::thread::sleep(std::time::Duration::from_secs(1));
        num * 2
    });
    
    println!("First call: {}", expensive_closure.value(5));
    println!("Second call: {}", expensive_closure.value(5)); // Cached!
}
```

### Event Handler Pattern
```rust
struct Button {
    click_handlers: Vec<Box<dyn Fn()>>,
}

impl Button {
    fn new() -> Self {
        Button {
            click_handlers: Vec::new(),
        }
    }
    
    fn on_click<F>(&mut self, handler: F)
    where
        F: Fn() + 'static,
    {
        self.click_handlers.push(Box::new(handler));
    }
    
    fn click(&self) {
        for handler in &self.click_handlers {
            handler();
        }
    }
}

fn main() {
    let mut button = Button::new();
    
    button.on_click(|| println!("Button clicked!"));
    button.on_click(|| println!("Another handler!"));
    
    button.click();
}
```

## Practical Use Cases

### Sorting with Custom Comparator
```rust
fn main() {
    let mut people = vec![
        ("Alice", 30),
        ("Bob", 25),
        ("Charlie", 35),
    ];
    
    // Sort by age
    people.sort_by(|a, b| a.1.cmp(&b.1));
    println!("By age: {:?}", people);
    
    // Sort by name
    people.sort_by(|a, b| a.0.cmp(&b.0));
    println!("By name: {:?}", people);
}
```

### Lazy Evaluation
```rust
fn main() {
    let expensive_calculation = || {
        println!("Performing expensive calculation...");
        std::thread::sleep(std::time::Duration::from_secs(1));
        42
    };
    
    let use_result = false;
    
    if use_result {
        let result = expensive_calculation();
        println!("Result: {}", result);
    } else {
        println!("Skipped calculation");
    }
}
```

## Best Practices

1. Use closures for short, simple functions
2. Prefer `move` when the closure outlives the current scope
3. Use explicit types when clarity is needed
4. Consider performance implications of capturing large values
5. Use function traits (`Fn`, `FnMut`, `FnOnce`) to be explicit about closure requirements