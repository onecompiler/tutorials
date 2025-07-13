# Slices in Rust

Slices are references to a contiguous sequence of elements in a collection. They allow you to reference part of a collection without taking ownership.

## String Slices

String slices (`&str`) are references to part of a `String`:

```rust
fn main() {
    let s = String::from("hello world");
    
    let hello = &s[0..5];  // "hello"
    let world = &s[6..11]; // "world"
    
    println!("First word: {}", hello);
    println!("Second word: {}", world);
}
```

### Slice Syntax

```rust
fn main() {
    let s = String::from("hello");
    
    let slice = &s[0..2]; // "he"
    let slice = &s[..2];  // Same as above
    
    let slice = &s[3..s.len()]; // "lo"
    let slice = &s[3..];        // Same as above
    
    let slice = &s[0..s.len()]; // "hello"
    let slice = &s[..];         // Same as above
}
```

## Array Slices

Slices work with arrays and vectors too:

```rust
fn main() {
    let a = [1, 2, 3, 4, 5];
    
    let slice = &a[1..3]; // [2, 3]
    
    println!("Slice: {:?}", slice);
    
    // Works with vectors
    let v = vec![10, 20, 30, 40, 50];
    let v_slice = &v[2..4]; // [30, 40]
    
    println!("Vector slice: {:?}", v_slice);
}
```

## Function Parameters

Slices as parameters make functions more flexible:

```rust
fn first_word(s: &str) -> &str {
    let bytes = s.as_bytes();
    
    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }
    
    &s[..]
}

fn main() {
    let my_string = String::from("hello world");
    
    // Works with String slices
    let word = first_word(&my_string[..]);
    println!("First word: {}", word);
    
    // Also works with string literals
    let literal = "hello world";
    let word = first_word(literal);
    println!("First word: {}", word);
}
```

## Mutable Slices

You can create mutable slices:

```rust
fn main() {
    let mut a = [1, 2, 3, 4, 5];
    let slice = &mut a[1..4]; // Mutable slice
    
    slice[0] = 10; // Modify through slice
    
    println!("Modified array: {:?}", a); // [1, 10, 3, 4, 5]
}
```

## Working with Slices

### Iterating Over Slices
```rust
fn main() {
    let a = [10, 20, 30, 40, 50];
    let slice = &a[1..4];
    
    for &item in slice {
        println!("Item: {}", item);
    }
}
```

### Slice Methods
```rust
fn main() {
    let numbers = [1, 2, 3, 4, 5];
    let slice = &numbers[..];
    
    println!("Length: {}", slice.len());
    println!("Is empty: {}", slice.is_empty());
    
    if let Some(&first) = slice.first() {
        println!("First: {}", first);
    }
    
    if let Some(&last) = slice.last() {
        println!("Last: {}", last);
    }
}
```

## Pattern Matching with Slices

```rust
fn main() {
    let numbers = [1, 2, 3, 4, 5];
    
    match &numbers[..] {
        [] => println!("Empty"),
        [single] => println!("Single element: {}", single),
        [first, second] => println!("Two elements: {}, {}", first, second),
        [first, .., last] => println!("First: {}, Last: {}", first, last),
    }
}
```

## String Literals Are Slices

String literals are actually slices:

```rust
fn main() {
    let s: &str = "Hello, world!"; // &str is a slice
    
    println!("String literal: {}", s);
    
    // This is why this works
    fn takes_slice(s: &str) {
        println!("{}", s);
    }
    
    takes_slice("literal"); // String literal
    takes_slice(&String::from("String")); // Deref coercion
}
```

## Common Slice Operations

### Splitting Strings
```rust
fn main() {
    let text = "hello,world,rust";
    
    for word in text.split(',') {
        println!("Word: {}", word);
    }
    
    let parts: Vec<&str> = text.split(',').collect();
    println!("Parts: {:?}", parts);
}
```

### Finding Substrings
```rust
fn main() {
    let s = "Hello, Rust!";
    
    if s.contains("Rust") {
        println!("Found Rust!");
    }
    
    if let Some(index) = s.find("Rust") {
        println!("Rust found at index: {}", index);
        let rust_slice = &s[index..];
        println!("Slice from Rust: {}", rust_slice);
    }
}
```

### Chunking Slices
```rust
fn main() {
    let data = [1, 2, 3, 4, 5, 6, 7, 8];
    
    for chunk in data.chunks(3) {
        println!("Chunk: {:?}", chunk);
    }
}
```

## Advanced Slice Patterns

### Windows
```rust
fn main() {
    let numbers = [1, 2, 3, 4, 5];
    
    for window in numbers.windows(3) {
        println!("Window: {:?}", window);
    }
}
```

### Split at Index
```rust
fn main() {
    let numbers = [1, 2, 3, 4, 5];
    
    let (left, right) = numbers.split_at(3);
    println!("Left: {:?}", left);   // [1, 2, 3]
    println!("Right: {:?}", right);  // [4, 5]
}
```

### Binary Search on Sorted Slices
```rust
fn main() {
    let sorted = [1, 3, 5, 7, 9, 11];
    
    match sorted.binary_search(&5) {
        Ok(index) => println!("Found 5 at index: {}", index),
        Err(index) => println!("5 should be inserted at index: {}", index),
    }
}
```

## Practical Examples

### Word Counter
```rust
fn count_words(text: &str) -> usize {
    text.split_whitespace().count()
}

fn main() {
    let text = "Hello world from Rust";
    println!("Word count: {}", count_words(text));
}
```

### Custom Split Function
```rust
fn split_at_char(s: &str, ch: char) -> (&str, &str) {
    if let Some(index) = s.find(ch) {
        (&s[..index], &s[index + 1..])
    } else {
        (s, "")
    }
}

fn main() {
    let email = "user@example.com";
    let (username, domain) = split_at_char(email, '@');
    
    println!("Username: {}", username);
    println!("Domain: {}", domain);
}
```

### Processing Data in Chunks
```rust
fn process_batch(data: &[i32]) -> i32 {
    data.iter().sum()
}

fn main() {
    let large_data = vec![1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
    let batch_size = 3;
    
    for (i, batch) in large_data.chunks(batch_size).enumerate() {
        let sum = process_batch(batch);
        println!("Batch {}: sum = {}", i, sum);
    }
}
```

## Best Practices

1. **Use `&str` parameters** instead of `&String` for more flexibility
2. **Return slices carefully** - ensure the data outlives the slice
3. **Prefer slices over indexing** for safety and clarity
4. **Use iterator methods** on slices for functional programming style
5. **Be careful with UTF-8** - string slicing can panic if not on char boundaries

## Common Pitfalls

### Invalid UTF-8 Boundaries
```rust
fn main() {
    let hello = "Здравствуйте"; // Russian "Hello"
    
    // This will panic - not on char boundary
    // let s = &hello[0..1];
    
    // Use char indices instead
    let s = &hello[0..2]; // First character (2 bytes)
    println!("First char: {}", s);
}
```

### Lifetime Issues
```rust
// This won't compile
// fn bad_slice() -> &[i32] {
//     let v = vec![1, 2, 3];
//     &v[..] // Error: v is dropped
// }

// Return owned data instead
fn good_slice() -> Vec<i32> {
    vec![1, 2, 3]
}
```