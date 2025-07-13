# Lifetimes in Rust

Lifetimes are Rust's way of ensuring references are valid for as long as they're used. Every reference has a lifetime, which is the scope for which that reference is valid.

## Understanding Lifetimes

### The Borrow Checker
```rust
fn main() {
    let r;                // ---------+-- 'a
                         //          |
    {                    //          |
        let x = 5;       // -+-- 'b  |
        r = &x;          //  |       |
    }                    // -+       |
                         //          |
    // println!("{}", r); // Error!  |
}                        // ---------+
```

## Lifetime Annotation Syntax

Lifetime annotations don't change how long references live - they describe the relationships between lifetimes.

```rust
&i32        // a reference
&'a i32     // a reference with an explicit lifetime
&'a mut i32 // a mutable reference with an explicit lifetime
```

## Function Lifetimes

### When Lifetimes Are Needed
```rust
// This won't compile without lifetime annotations
// fn longest(x: &str, y: &str) -> &str {
//     if x.len() > y.len() { x } else { y }
// }

// With lifetime annotations
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}

fn main() {
    let string1 = String::from("long string");
    let string2 = "xyz";
    
    let result = longest(string1.as_str(), string2);
    println!("Longest: {}", result);
}
```

### Different Lifetimes
```rust
fn first_word<'a>(s: &'a str) -> &'a str {
    let bytes = s.as_bytes();
    
    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }
    
    &s[..]
}

// Multiple lifetimes
fn longest_with_announcement<'a, 'b>(
    x: &'a str,
    y: &'a str,
    ann: &'b str,
) -> &'a str {
    println!("Announcement: {}", ann);
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```

## Lifetime Elision Rules

The compiler uses three rules to infer lifetimes:

1. Each input reference gets its own lifetime
2. If there's one input lifetime, it's assigned to all outputs
3. If there's `&self` or `&mut self`, its lifetime is assigned to outputs

```rust
// Original
fn first_word(s: &str) -> &str {
    // ...
}

// After rule 1
fn first_word<'a>(s: &'a str) -> &str {
    // ...
}

// After rule 2
fn first_word<'a>(s: &'a str) -> &'a str {
    // ...
}
```

## Struct Lifetimes

Structs can hold references with lifetime annotations:

```rust
struct ImportantExcerpt<'a> {
    part: &'a str,
}

impl<'a> ImportantExcerpt<'a> {
    fn level(&self) -> i32 {
        3
    }
    
    fn announce_and_return_part(&self, announcement: &str) -> &str {
        println!("Attention: {}", announcement);
        self.part
    }
}

fn main() {
    let novel = String::from("Call me Ishmael. Some years ago...");
    let first_sentence = novel.split('.').next().expect("Could not find a '.'");
    
    let excerpt = ImportantExcerpt {
        part: first_sentence,
    };
    
    println!("Excerpt: {}", excerpt.part);
}
```

## Static Lifetime

The `'static` lifetime means the reference can live for the entire program:

```rust
fn main() {
    let s: &'static str = "I have a static lifetime.";
    
    // String literals have 'static lifetime
    let literal = "Hello, world!";
    
    // This function requires 'static
    fn needs_static(s: &'static str) {
        println!("Static string: {}", s);
    }
    
    needs_static(literal);
}
```

## Advanced Lifetime Patterns

### Lifetime Bounds
```rust
use std::fmt::Display;

fn longest_with_display<'a, T>(x: &'a str, y: &'a str, ann: T) -> &'a str
where
    T: Display,
{
    println!("Announcement: {}", ann);
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```

### Multiple Lifetime Parameters
```rust
struct Context<'s> {
    text: &'s str,
}

struct Parser<'c, 's> {
    context: &'c Context<'s>,
}

impl<'c, 's> Parser<'c, 's> {
    fn parse(&self) -> Result<(), &'s str> {
        Err(&self.context.text[1..])
    }
}

fn parse_context(context: Context) -> Result<(), &str> {
    Parser { context: &context }.parse()
}
```

### Lifetime Subtyping
```rust
struct Context<'a> {
    text: &'a str,
}

// 'a: 'b means 'a lives at least as long as 'b
impl<'a: 'b, 'b> Context<'a> {
    fn get_ref(&'b self) -> &'a str {
        self.text
    }
}
```

## Common Lifetime Scenarios

### Returning References from Functions
```rust
// Can't return reference to local variable
// fn dangle() -> &String {
//     let s = String::from("hello");
//     &s // Error: s goes out of scope
// }

// Return owned value instead
fn no_dangle() -> String {
    let s = String::from("hello");
    s
}

// Or accept reference and return part of it
fn first_word<'a>(s: &'a str) -> &'a str {
    s.split_whitespace().next().unwrap_or("")
}
```

### Mutable References
```rust
fn change_and_return<'a>(s: &'a mut String) -> &'a mut String {
    s.push_str(" world");
    s
}

fn main() {
    let mut s = String::from("hello");
    let s_ref = change_and_return(&mut s);
    println!("{}", s_ref);
}
```

## Practical Examples

### String Manipulation
```rust
struct StringSplitter<'a> {
    remainder: &'a str,
    delimiter: &'a str,
}

impl<'a> StringSplitter<'a> {
    fn new(string: &'a str, delimiter: &'a str) -> Self {
        StringSplitter {
            remainder: string,
            delimiter,
        }
    }
}

impl<'a> Iterator for StringSplitter<'a> {
    type Item = &'a str;
    
    fn next(&mut self) -> Option<Self::Item> {
        if let Some(index) = self.remainder.find(self.delimiter) {
            let part = &self.remainder[..index];
            self.remainder = &self.remainder[index + self.delimiter.len()..];
            Some(part)
        } else if !self.remainder.is_empty() {
            let part = self.remainder;
            self.remainder = "";
            Some(part)
        } else {
            None
        }
    }
}

fn main() {
    let text = "a,b,c,d";
    let splitter = StringSplitter::new(text, ",");
    
    for part in splitter {
        println!("Part: {}", part);
    }
}
```

### Cache with Lifetime
```rust
use std::collections::HashMap;

struct Cache<'a> {
    data: HashMap<&'a str, &'a str>,
}

impl<'a> Cache<'a> {
    fn new() -> Self {
        Cache {
            data: HashMap::new(),
        }
    }
    
    fn insert(&mut self, key: &'a str, value: &'a str) {
        self.data.insert(key, value);
    }
    
    fn get(&self, key: &'a str) -> Option<&&'a str> {
        self.data.get(key)
    }
}

fn main() {
    let key = String::from("name");
    let value = String::from("Alice");
    
    let mut cache = Cache::new();
    cache.insert(&key, &value);
    
    if let Some(v) = cache.get(&key) {
        println!("Found: {}", v);
    }
}
```

## Best Practices

1. **Let the compiler infer** lifetimes when possible
2. **Use lifetime elision** - don't annotate unnecessarily
3. **Name lifetimes meaningfully** when multiple are involved
4. **Consider ownership** instead of complex lifetimes
5. **Use `'static` sparingly** - it's often not what you want
6. **Think about lifetime relationships** not absolute durations
7. **Simplify by returning owned values** when lifetimes get complex
8. **Document lifetime requirements** in complex APIs

## Common Errors and Solutions

### Lifetime Mismatch
```rust
// Error: lifetime mismatch
// fn invalid<'a>() -> &'a str {
//     let s = String::from("hello");
//     &s // s doesn't live long enough
// }

// Solution: return owned value
fn valid() -> String {
    String::from("hello")
}
```

### Multiple Possible Lifetimes
```rust
// Error: compiler can't determine lifetime
// fn longest(x: &str, y: &str) -> &str {
//     if x.len() > y.len() { x } else { y }
// }

// Solution: explicit lifetime annotation
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() { x } else { y }
}
```