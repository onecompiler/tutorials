# Strings in Rust

Rust has two main string types: `String` (owned, growable) and `&str` (borrowed, fixed-size). Understanding both is crucial for effective Rust programming.

## String Types

### String vs &str
```rust
fn main() {
    // String - heap allocated, owned, mutable
    let mut owned = String::from("Hello");
    owned.push_str(", World!");
    
    // &str - string slice, borrowed, immutable
    let borrowed: &str = "Hello, World!";
    
    // String literal is &'static str
    let literal = "I live for the entire program";
}
```

## Creating Strings

### Various Ways to Create Strings
```rust
fn main() {
    // From string literal
    let s1 = String::from("Hello");
    let s2 = "Hello".to_string();
    let s3 = "Hello".to_owned();
    
    // Empty string
    let mut s4 = String::new();
    s4.push_str("Hello");
    
    // With capacity
    let s5 = String::with_capacity(25);
    println!("Capacity: {}", s5.capacity());
    
    // From other types
    let s6 = 42.to_string();
    let s7 = format!("The answer is {}", 42);
}
```

## String Operations

### Concatenation
```rust
fn main() {
    let mut s = String::from("Hello");
    
    // Push string slice
    s.push_str(", ");
    
    // Push single character
    s.push('W');
    s.push_str("orld!");
    
    println!("{}", s);
    
    // Using + operator (moves first string)
    let s1 = String::from("Hello, ");
    let s2 = String::from("World!");
    let s3 = s1 + &s2; // s1 is moved
    println!("{}", s3);
    
    // Using format! (doesn't take ownership)
    let s1 = String::from("tic");
    let s2 = String::from("tac");
    let s3 = String::from("toe");
    let s = format!("{}-{}-{}", s1, s2, s3);
    println!("{}", s);
}
```

### Indexing and Slicing

Rust strings are UTF-8 encoded, so indexing is not straightforward:

```rust
fn main() {
    let hello = String::from("Hello");
    
    // This won't compile - no indexing!
    // let c = hello[0];
    
    // Use chars() for iteration
    let first_char = hello.chars().nth(0);
    println!("First char: {:?}", first_char);
    
    // Slicing (be careful with boundaries)
    let slice = &hello[0..2];
    println!("Slice: {}", slice);
    
    // UTF-8 example
    let hello = "Здравствуйте";
    // let s = &hello[0..1]; // PANIC! Not char boundary
    let s = &hello[0..2]; // First character (2 bytes)
    println!("First char: {}", s);
}
```

### Iteration

```rust
fn main() {
    let text = "Hello, 世界";
    
    // Iterate over characters
    for c in text.chars() {
        println!("Char: {}", c);
    }
    
    // Iterate over bytes
    for b in text.bytes() {
        println!("Byte: {}", b);
    }
    
    // Iterate over grapheme clusters (requires external crate)
    // Most accurate for human-perceived characters
}
```

## String Methods

### Searching
```rust
fn main() {
    let text = "Hello, World!";
    
    // Check if contains
    if text.contains("World") {
        println!("Found World!");
    }
    
    // Find position
    if let Some(index) = text.find("World") {
        println!("World starts at index: {}", index);
    }
    
    // Starts/ends with
    println!("Starts with Hello: {}", text.starts_with("Hello"));
    println!("Ends with !: {}", text.ends_with("!"));
}
```

### Replacing
```rust
fn main() {
    let text = "Hello, World!";
    
    // Replace all occurrences
    let new_text = text.replace("World", "Rust");
    println!("{}", new_text);
    
    // Replace first n occurrences
    let text = "foo foo foo";
    let new_text = text.replacen("foo", "bar", 2);
    println!("{}", new_text); // "bar bar foo"
}
```

### Trimming
```rust
fn main() {
    let text = "  Hello, World!  \n";
    
    // Trim whitespace
    println!("'{}'", text.trim());
    
    // Trim specific characters
    let text = "...Hello...";
    println!("'{}'", text.trim_matches('.'));
    
    // Trim start/end only
    let text = "  Hello  ";
    println!("'{}'", text.trim_start());
    println!("'{}'", text.trim_end());
}
```

### Case Conversion
```rust
fn main() {
    let text = "Hello, World!";
    
    println!("Uppercase: {}", text.to_uppercase());
    println!("Lowercase: {}", text.to_lowercase());
    
    // ASCII only (faster)
    let text = "Hello";
    println!("ASCII uppercase: {}", text.to_ascii_uppercase());
}
```

### Splitting
```rust
fn main() {
    let text = "apple,banana,orange";
    
    // Split by delimiter
    for fruit in text.split(',') {
        println!("Fruit: {}", fruit);
    }
    
    // Split and collect
    let fruits: Vec<&str> = text.split(',').collect();
    println!("Fruits: {:?}", fruits);
    
    // Split whitespace
    let text = "Hello   World  Rust";
    let words: Vec<&str> = text.split_whitespace().collect();
    println!("Words: {:?}", words);
    
    // Split with limit
    let parts: Vec<&str> = text.splitn(2, ' ').collect();
    println!("First two parts: {:?}", parts);
}
```

## String Parsing

```rust
use std::str::FromStr;

fn main() {
    // Parse to number
    let num_str = "42";
    let num: i32 = num_str.parse().expect("Not a number");
    println!("Parsed: {}", num);
    
    // Parse with turbofish
    let num = "3.14".parse::<f64>().unwrap();
    println!("Float: {}", num);
    
    // Custom parsing
    #[derive(Debug)]
    struct Point {
        x: i32,
        y: i32,
    }
    
    impl FromStr for Point {
        type Err = String;
        
        fn from_str(s: &str) -> Result<Self, Self::Err> {
            let parts: Vec<&str> = s.split(',').collect();
            if parts.len() != 2 {
                return Err("Invalid format".to_string());
            }
            
            let x = parts[0].parse().map_err(|_| "Invalid x")?;
            let y = parts[1].parse().map_err(|_| "Invalid y")?;
            
            Ok(Point { x, y })
        }
    }
    
    let point: Point = "10,20".parse().unwrap();
    println!("Point: {:?}", point);
}
```

## UTF-8 and Unicode

```rust
fn main() {
    let hello = "Hello, 世界! 🌍";
    
    // Length in bytes
    println!("Bytes: {}", hello.len());
    
    // Length in characters
    println!("Chars: {}", hello.chars().count());
    
    // Iterate with indices
    for (i, c) in hello.char_indices() {
        println!("{}: {}", i, c);
    }
    
    // Check if valid UTF-8
    let bytes = vec![72, 101, 108, 108, 111];
    match String::from_utf8(bytes) {
        Ok(s) => println!("Valid UTF-8: {}", s),
        Err(e) => println!("Invalid UTF-8: {}", e),
    }
}
```

## Practical Examples

### String Builder Pattern
```rust
fn build_query(params: Vec<(&str, &str)>) -> String {
    let mut query = String::new();
    
    for (i, (key, value)) in params.iter().enumerate() {
        if i > 0 {
            query.push('&');
        }
        query.push_str(&format!("{}={}", key, value));
    }
    
    query
}

fn main() {
    let params = vec![("name", "Alice"), ("age", "30"), ("city", "NYC")];
    let query = build_query(params);
    println!("Query: {}", query);
}
```

### Word Counter
```rust
use std::collections::HashMap;

fn count_words(text: &str) -> HashMap<String, u32> {
    let mut counts = HashMap::new();
    
    for word in text.split_whitespace() {
        let word = word.to_lowercase();
        *counts.entry(word).or_insert(0) += 1;
    }
    
    counts
}

fn main() {
    let text = "The quick brown fox jumps over the lazy fox";
    let counts = count_words(text);
    
    for (word, count) in counts {
        println!("{}: {}", word, count);
    }
}
```

### Template Engine
```rust
fn render_template(template: &str, vars: Vec<(&str, &str)>) -> String {
    let mut result = template.to_string();
    
    for (key, value) in vars {
        let placeholder = format!("{{{}}}", key);
        result = result.replace(&placeholder, value);
    }
    
    result
}

fn main() {
    let template = "Hello {name}, you are {age} years old!";
    let vars = vec![("name", "Alice"), ("age", "30")];
    
    let rendered = render_template(template, vars);
    println!("{}", rendered);
}
```

## Best Practices

1. **Use `&str` for function parameters** when you don't need ownership
2. **Use `String` when you need to own or modify** the string
3. **Be careful with string slicing** - respect UTF-8 boundaries
4. **Use `format!` for complex string building**
5. **Prefer `push_str` over `+`** for multiple concatenations
6. **Use `chars()` for character iteration**, not indexing
7. **Consider `Cow<str>`** for functions that might or might not allocate
8. **Pre-allocate with `String::with_capacity`** when size is known