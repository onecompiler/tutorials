# Ownership in Rust

Ownership is Rust's most unique feature, enabling memory safety without a garbage collector. Understanding ownership is crucial for writing effective Rust code.

## The Three Rules of Ownership

1. Each value in Rust has a single owner
2. There can only be one owner at a time
3. When the owner goes out of scope, the value is dropped

## Stack vs Heap

Understanding where data lives helps understand ownership:

```rust
fn main() {
    // Stack data - fixed size, fast
    let x = 5;           // i32 on stack
    let y = true;        // bool on stack
    
    // Heap data - dynamic size, slower
    let s = String::from("hello"); // String on heap
    let v = vec![1, 2, 3];        // Vec on heap
}
```

## Move Semantics

When assigning heap data, ownership is moved:

```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1; // s1 is moved to s2
    
    // println!("{}", s1); // Error! s1 is no longer valid
    println!("{}", s2); // This works
}
```

### Copy Types

Some types implement `Copy` and are duplicated instead of moved:

```rust
fn main() {
    let x = 5;
    let y = x; // x is copied to y
    
    println!("x: {}, y: {}", x, y); // Both are valid
}
```

Types that implement `Copy`:
- All integer types
- Boolean type
- Floating point types
- Character type
- Tuples containing only Copy types

## Ownership and Functions

Passing values to functions transfers ownership:

```rust
fn main() {
    let s = String::from("hello");
    
    takes_ownership(s); // s is moved into the function
    
    // println!("{}", s); // Error! s is no longer valid
    
    let x = 5;
    
    makes_copy(x); // x is copied
    
    println!("x is still {}", x); // This works
}

fn takes_ownership(some_string: String) {
    println!("{}", some_string);
} // some_string goes out of scope and is dropped

fn makes_copy(some_integer: i32) {
    println!("{}", some_integer);
}
```

## Returning Values

Functions can transfer ownership back:

```rust
fn main() {
    let s1 = gives_ownership(); // Function transfers ownership to s1
    
    let s2 = String::from("hello");
    let s3 = takes_and_gives_back(s2); // s2 is moved in, s3 gets ownership
    
    println!("s1: {}", s1);
    println!("s3: {}", s3);
    // println!("s2: {}", s2); // Error! s2 was moved
}

fn gives_ownership() -> String {
    let some_string = String::from("yours");
    some_string // Ownership is moved out
}

fn takes_and_gives_back(a_string: String) -> String {
    a_string // Return ownership
}
```

## Clone

To deeply copy heap data, use `clone()`:

```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1.clone(); // Deep copy
    
    println!("s1: {}, s2: {}", s1, s2); // Both are valid
}
```

## Drop Trait

When a value goes out of scope, Rust calls `drop`:

```rust
struct CustomData {
    data: String,
}

impl Drop for CustomData {
    fn drop(&mut self) {
        println!("Dropping CustomData with data: {}", self.data);
    }
}

fn main() {
    {
        let c = CustomData {
            data: String::from("my data"),
        };
        println!("CustomData created");
    } // c goes out of scope, drop is called
    
    println!("After scope");
}
```

## Ownership in Collections

Collections own their elements:

```rust
fn main() {
    let v = vec![String::from("hello"), String::from("world")];
    
    // Moving out of vector
    let first = v[0].clone(); // Must clone to avoid moving
    println!("First: {}", first);
    
    // Iterating transfers ownership
    for s in v {
        println!("{}", s);
    }
    // v is no longer accessible
}
```

## Common Ownership Patterns

### Builder Pattern
```rust
struct Config {
    name: String,
    value: i32,
}

impl Config {
    fn new() -> Self {
        Config {
            name: String::new(),
            value: 0,
        }
    }
    
    fn name(mut self, name: String) -> Self {
        self.name = name;
        self // Return ownership
    }
    
    fn value(mut self, value: i32) -> Self {
        self.value = value;
        self // Return ownership
    }
}

fn main() {
    let config = Config::new()
        .name(String::from("test"))
        .value(42);
    
    println!("Config: {} = {}", config.name, config.value);
}
```

### Option for Nullable Ownership
```rust
fn main() {
    let mut optional_string: Option<String> = Some(String::from("hello"));
    
    // Take ownership out of Option
    if let Some(s) = optional_string.take() {
        println!("Took: {}", s);
    }
    
    println!("Option is now: {:?}", optional_string); // None
}
```

## Memory Safety Examples

### Preventing Use After Free
```rust
fn main() {
    let reference;
    {
        let data = String::from("hello");
        // reference = &data; // Error! data doesn't live long enough
    }
    // println!("{}", reference); // Would be use-after-free
}
```

### Preventing Double Free
```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1; // Ownership moved, not copied
    
    // Only s2 will be dropped, preventing double free
}
```

## Best Practices

1. **Prefer moving over cloning** when possible for performance
2. **Use references** (borrowing) when you don't need ownership
3. **Return owned values** from functions when creating new data
4. **Use `String` for owned strings**, `&str` for borrowed strings
5. **Consider `Rc` or `Arc`** for shared ownership when needed

## Common Pitfalls

### Trying to Use Moved Values
```rust
fn consume(s: String) {
    println!("{}", s);
}

fn main() {
    let s = String::from("hello");
    consume(s);
    // consume(s); // Error! s was moved
}
```

### Moving in Loops
```rust
fn main() {
    let data = vec![String::from("a"), String::from("b")];
    
    // This moves data
    for s in data {
        println!("{}", s);
    }
    
    // println!("{:?}", data); // Error! data was moved
    
    // Use reference to avoid moving
    let data2 = vec![String::from("a"), String::from("b")];
    for s in &data2 {
        println!("{}", s);
    }
    println!("{:?}", data2); // This works
}
```

## Debugging Ownership

Use these techniques to understand ownership:

```rust
fn print_type_of<T>(_: &T) {
    println!("{}", std::any::type_name::<T>());
}

fn main() {
    let s = String::from("hello");
    print_type_of(&s); // Borrow to check type without moving
    
    // Check if Copy
    let x = 5;
    let y = x;
    println!("x: {}, y: {}", x, y); // Both valid means Copy
}
```