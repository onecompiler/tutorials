# Traits in Rust

Traits are Rust's way of defining shared behavior. They're similar to interfaces in other languages but more powerful.

## Defining Traits

A trait defines a set of methods that types can implement:

```rust
trait Animal {
    fn name(&self) -> &str;
    fn speak(&self);
    
    // Default implementation
    fn description(&self) -> String {
        format!("{} says something", self.name())
    }
}
```

## Implementing Traits

```rust
struct Dog {
    name: String,
}

struct Cat {
    name: String,
}

impl Animal for Dog {
    fn name(&self) -> &str {
        &self.name
    }
    
    fn speak(&self) {
        println!("{} says Woof!", self.name());
    }
}

impl Animal for Cat {
    fn name(&self) -> &str {
        &self.name
    }
    
    fn speak(&self) {
        println!("{} says Meow!", self.name());
    }
    
    // Override default implementation
    fn description(&self) -> String {
        format!("{} is a cat that meows", self.name())
    }
}

fn main() {
    let dog = Dog { name: String::from("Rex") };
    let cat = Cat { name: String::from("Whiskers") };
    
    dog.speak();
    cat.speak();
    
    println!("{}", dog.description());
    println!("{}", cat.description());
}
```

## Trait Bounds

Specify that a generic type must implement certain traits:

```rust
// Function with trait bound
fn animal_speak<T: Animal>(animal: &T) {
    animal.speak();
}

// Multiple bounds
fn process<T: Animal + Clone>(animal: &T) {
    let cloned = animal.clone();
    animal.speak();
}

// Where clause for complex bounds
fn complex_function<T, U>(t: &T, u: &U) 
where
    T: Animal + Clone,
    U: Animal + Debug,
{
    t.speak();
    u.speak();
}
```

## Common Standard Traits

### Display and Debug
```rust
use std::fmt;

struct Point {
    x: i32,
    y: i32,
}

impl fmt::Display for Point {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "({}, {})", self.x, self.y)
    }
}

impl fmt::Debug for Point {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        f.debug_struct("Point")
            .field("x", &self.x)
            .field("y", &self.y)
            .finish()
    }
}

fn main() {
    let p = Point { x: 10, y: 20 };
    println!("Display: {}", p);    // (10, 20)
    println!("Debug: {:?}", p);    // Point { x: 10, y: 20 }
}
```

### Clone and Copy
```rust
#[derive(Clone, Copy)]
struct Point {
    x: i32,
    y: i32,
}

#[derive(Clone)]
struct Container {
    data: Vec<i32>,
}

fn main() {
    let p1 = Point { x: 10, y: 20 };
    let p2 = p1; // Copy
    println!("p1: {:?}, p2: {:?}", p1, p2); // Both valid
    
    let c1 = Container { data: vec![1, 2, 3] };
    let c2 = c1.clone(); // Explicit clone
    println!("c2: {:?}", c2);
}
```

### PartialEq and Eq
```rust
#[derive(PartialEq, Eq)]
struct Person {
    name: String,
    age: u32,
}

fn main() {
    let p1 = Person { name: String::from("Alice"), age: 30 };
    let p2 = Person { name: String::from("Alice"), age: 30 };
    let p3 = Person { name: String::from("Bob"), age: 25 };
    
    println!("p1 == p2: {}", p1 == p2); // true
    println!("p1 == p3: {}", p1 == p3); // false
}
```

### PartialOrd and Ord
```rust
#[derive(PartialEq, Eq, PartialOrd, Ord)]
struct Version {
    major: u32,
    minor: u32,
    patch: u32,
}

fn main() {
    let v1 = Version { major: 1, minor: 2, patch: 3 };
    let v2 = Version { major: 1, minor: 3, patch: 0 };
    
    println!("v1 < v2: {}", v1 < v2); // true
    
    let mut versions = vec![
        Version { major: 2, minor: 0, patch: 0 },
        Version { major: 1, minor: 0, patch: 0 },
        Version { major: 1, minor: 2, patch: 0 },
    ];
    
    versions.sort();
    println!("Sorted: {:?}", versions);
}
```

## Trait Objects

Dynamic dispatch using trait objects:

```rust
trait Draw {
    fn draw(&self);
}

struct Circle {
    radius: f64,
}

struct Rectangle {
    width: f64,
    height: f64,
}

impl Draw for Circle {
    fn draw(&self) {
        println!("Drawing circle with radius {}", self.radius);
    }
}

impl Draw for Rectangle {
    fn draw(&self) {
        println!("Drawing rectangle {}x{}", self.width, self.height);
    }
}

fn main() {
    // Trait objects
    let shapes: Vec<Box<dyn Draw>> = vec![
        Box::new(Circle { radius: 5.0 }),
        Box::new(Rectangle { width: 10.0, height: 20.0 }),
    ];
    
    for shape in shapes {
        shape.draw();
    }
}
```

## Associated Types

Traits can have associated types:

```rust
trait Container {
    type Item;
    
    fn add(&mut self, item: Self::Item);
    fn get(&self, index: usize) -> Option<&Self::Item>;
}

struct VecContainer<T> {
    items: Vec<T>,
}

impl<T> Container for VecContainer<T> {
    type Item = T;
    
    fn add(&mut self, item: Self::Item) {
        self.items.push(item);
    }
    
    fn get(&self, index: usize) -> Option<&Self::Item> {
        self.items.get(index)
    }
}
```

## Default Implementations

Provide default behavior that can be overridden:

```rust
trait Summary {
    fn summarize_author(&self) -> String;
    
    // Default implementation
    fn summarize(&self) -> String {
        format!("(Read more from {}...)", self.summarize_author())
    }
}

struct Article {
    author: String,
    content: String,
}

impl Summary for Article {
    fn summarize_author(&self) -> String {
        self.author.clone()
    }
    
    // Use default summarize()
}

struct Tweet {
    username: String,
    content: String,
}

impl Summary for Tweet {
    fn summarize_author(&self) -> String {
        format!("@{}", self.username)
    }
    
    // Override default
    fn summarize(&self) -> String {
        format!("{}: {}", self.summarize_author(), self.content)
    }
}
```

## Supertraits

Traits that require other traits:

```rust
use std::fmt;

trait OutlinePrint: fmt::Display {
    fn outline_print(&self) {
        let output = self.to_string();
        let len = output.len();
        
        println!("{}", "*".repeat(len + 4));
        println!("* {} *", output);
        println!("{}", "*".repeat(len + 4));
    }
}

impl fmt::Display for Point {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "({}, {})", self.x, self.y)
    }
}

impl OutlinePrint for Point {}

fn main() {
    let p = Point { x: 1, y: 3 };
    p.outline_print();
}
```

## Blanket Implementations

Implement a trait for all types that satisfy certain bounds:

```rust
trait MyTrait {
    fn do_something(&self);
}

// Blanket implementation
impl<T: Display> MyTrait for T {
    fn do_something(&self) {
        println!("Value: {}", self);
    }
}

fn main() {
    42.do_something();           // Works for i32
    "hello".do_something();      // Works for &str
    String::from("world").do_something(); // Works for String
}
```

## Practical Examples

### Custom Iterator
```rust
struct Counter {
    count: u32,
    max: u32,
}

impl Counter {
    fn new(max: u32) -> Self {
        Counter { count: 0, max }
    }
}

impl Iterator for Counter {
    type Item = u32;
    
    fn next(&mut self) -> Option<Self::Item> {
        if self.count < self.max {
            self.count += 1;
            Some(self.count)
        } else {
            None
        }
    }
}

fn main() {
    let counter = Counter::new(5);
    
    for num in counter {
        println!("Count: {}", num);
    }
    
    // Using iterator methods
    let sum: u32 = Counter::new(5).sum();
    println!("Sum: {}", sum);
}
```

### Plugin System
```rust
trait Plugin {
    fn name(&self) -> &str;
    fn execute(&self, input: &str) -> String;
}

struct UppercasePlugin;
struct ReversePlugin;

impl Plugin for UppercasePlugin {
    fn name(&self) -> &str {
        "Uppercase"
    }
    
    fn execute(&self, input: &str) -> String {
        input.to_uppercase()
    }
}

impl Plugin for ReversePlugin {
    fn name(&self) -> &str {
        "Reverse"
    }
    
    fn execute(&self, input: &str) -> String {
        input.chars().rev().collect()
    }
}

fn run_plugins(input: &str, plugins: Vec<Box<dyn Plugin>>) {
    for plugin in plugins {
        println!("{}: {}", plugin.name(), plugin.execute(input));
    }
}

fn main() {
    let plugins: Vec<Box<dyn Plugin>> = vec![
        Box::new(UppercasePlugin),
        Box::new(ReversePlugin),
    ];
    
    run_plugins("hello", plugins);
}
```

## Best Practices

1. **Keep traits focused** on a single responsibility
2. **Provide default implementations** when sensible
3. **Use associated types** instead of generics when there's only one logical type
4. **Derive common traits** when possible
5. **Document trait requirements** and invariants
6. **Consider object safety** if you need trait objects
7. **Use supertraits** to build on existing functionality
8. **Prefer static dispatch** (generics) over dynamic (trait objects) for performance