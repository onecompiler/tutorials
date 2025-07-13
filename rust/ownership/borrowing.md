# References and Borrowing in Rust

References allow you to refer to values without taking ownership. This is called borrowing in Rust.

## Creating References

Use `&` to create a reference:

```rust
fn main() {
    let s1 = String::from("hello");
    let len = calculate_length(&s1); // Borrow s1
    
    println!("The length of '{}' is {}.", s1, len); // s1 is still valid
}

fn calculate_length(s: &String) -> usize {
    s.len()
} // s goes out of scope but doesn't drop the String
```

## Immutable References

By default, references are immutable:

```rust
fn main() {
    let s = String::from("hello");
    
    change(&s);
    println!("{}", s);
}

fn change(s: &String) {
    // s.push_str(", world"); // Error! Cannot modify immutable reference
    println!("I can read: {}", s); // Reading is fine
}
```

## Mutable References

Use `&mut` for mutable references:

```rust
fn main() {
    let mut s = String::from("hello");
    
    change(&mut s);
    println!("{}", s); // Prints: hello, world
}

fn change(s: &mut String) {
    s.push_str(", world");
}
```

## The Rules of References

1. You can have either one mutable reference OR any number of immutable references
2. References must always be valid

### Multiple Immutable References Are Allowed
```rust
fn main() {
    let s = String::from("hello");
    
    let r1 = &s;
    let r2 = &s;
    let r3 = &s;
    
    println!("{}, {}, {}", r1, r2, r3); // All valid
}
```

### Cannot Mix Mutable and Immutable
```rust
fn main() {
    let mut s = String::from("hello");
    
    let r1 = &s; // Immutable borrow
    let r2 = &s; // Another immutable borrow
    // let r3 = &mut s; // Error! Cannot borrow as mutable
    
    println!("{}, {}", r1, r2);
}
```

### Only One Mutable Reference
```rust
fn main() {
    let mut s = String::from("hello");
    
    let r1 = &mut s;
    // let r2 = &mut s; // Error! Second mutable borrow
    
    println!("{}", r1);
}
```

## Reference Scope

References have scopes based on usage:

```rust
fn main() {
    let mut s = String::from("hello");
    
    let r1 = &s;
    let r2 = &s;
    println!("{} and {}", r1, r2);
    // r1 and r2 are no longer used after this point
    
    let r3 = &mut s; // This is okay
    println!("{}", r3);
}
```

## Dangling References

Rust prevents dangling references:

```rust
fn main() {
    // let reference = dangle(); // Won't compile
}

// This function won't compile
// fn dangle() -> &String {
//     let s = String::from("hello");
//     &s // Error! s is dropped, reference would be dangling
// }

// Correct version - return owned String
fn no_dangle() -> String {
    let s = String::from("hello");
    s // Ownership is moved out
}
```

## Method Receivers

Methods can borrow `self` in different ways:

```rust
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    // Immutable borrow
    fn area(&self) -> u32 {
        self.width * self.height
    }
    
    // Mutable borrow
    fn double_size(&mut self) {
        self.width *= 2;
        self.height *= 2;
    }
    
    // Take ownership
    fn destroy(self) {
        println!("Rectangle destroyed!");
    }
}

fn main() {
    let mut rect = Rectangle { width: 30, height: 50 };
    
    println!("Area: {}", rect.area()); // Immutable borrow
    rect.double_size(); // Mutable borrow
    println!("New area: {}", rect.area());
    
    rect.destroy(); // Takes ownership
    // rect.area(); // Error! rect was moved
}
```

## Borrowing in Patterns

### Borrowing Struct Fields
```rust
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let p = Point { x: 10, y: 20 };
    
    let Point { x: ref a, y: ref b } = p;
    println!("a: {}, b: {}", a, b);
    
    // p is still valid
    println!("Point: ({}, {})", p.x, p.y);
}
```

### Borrowing in Match
```rust
fn main() {
    let s = Some(String::from("hello"));
    
    match &s {
        Some(text) => println!("Found: {}", text),
        None => println!("Nothing"),
    }
    
    // s is still valid
    println!("{:?}", s);
}
```

## Explicit Lifetimes

Sometimes you need to specify lifetimes:

```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}

fn main() {
    let string1 = String::from("long string");
    let string2 = String::from("xyz");
    
    let result = longest(string1.as_str(), string2.as_str());
    println!("Longest: {}", result);
}
```

## Smart Pointers and Borrowing

### Box<T>
```rust
fn main() {
    let b = Box::new(5);
    let r = &*b; // Borrow the value inside Box
    
    println!("b: {}, r: {}", b, r);
}
```

### Interior Mutability with RefCell
```rust
use std::cell::RefCell;

fn main() {
    let data = RefCell::new(5);
    
    {
        let mut r = data.borrow_mut();
        *r += 1;
    } // Mutable borrow ends
    
    println!("Value: {}", data.borrow());
}
```

## Common Patterns

### Borrowing in Loops
```rust
fn main() {
    let numbers = vec![1, 2, 3, 4, 5];
    
    // Borrow for iteration
    for num in &numbers {
        println!("{}", num);
    }
    
    // numbers is still valid
    println!("Length: {}", numbers.len());
}
```

### Temporary Borrows
```rust
fn main() {
    let mut v = vec![1, 2, 3];
    
    // Temporary mutable borrow
    v.push(4);
    
    // Can borrow again
    let first = &v[0];
    println!("First: {}", first);
}
```

### Reborrowing
```rust
fn foo(s: &mut String) {
    bar(s); // Reborrow happens automatically
    s.push_str(" more");
}

fn bar(s: &mut String) {
    s.push_str(" text");
}

fn main() {
    let mut s = String::from("hello");
    foo(&mut s);
    println!("{}", s);
}
```

## Best Practices

1. **Prefer borrowing over ownership transfer** when you don't need to consume the value
2. **Use immutable references by default**, mutable only when needed
3. **Keep borrow scopes as small as possible**
4. **Return references carefully** - ensure they outlive their usage
5. **Use methods that borrow** instead of consuming when possible

## Common Mistakes

### Trying to Return Local References
```rust
// Won't compile
// fn bad() -> &str {
//     let s = String::from("hello");
//     &s // Error: returns reference to local variable
// }

// Fix: Return owned value
fn good() -> String {
    String::from("hello")
}
```

### Holding References Too Long
```rust
fn main() {
    let mut v = vec![1, 2, 3];
    
    let first = &v[0]; // Immutable borrow
    
    // v.push(4); // Error! Can't mutate while borrowed
    
    println!("First: {}", first);
    
    v.push(4); // OK after borrow ends
}
```