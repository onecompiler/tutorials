# HashMaps in Rust

HashMap is Rust's built-in hash table implementation, storing key-value pairs for efficient lookups.

## Creating HashMaps

```rust
use std::collections::HashMap;

fn main() {
    // Create empty HashMap
    let mut scores = HashMap::new();
    
    // Insert values
    scores.insert(String::from("Blue"), 10);
    scores.insert(String::from("Red"), 50);
    
    // Create from arrays
    let teams = vec![String::from("Blue"), String::from("Red")];
    let initial_scores = vec![10, 50];
    
    let scores: HashMap<_, _> = teams.into_iter()
        .zip(initial_scores.into_iter())
        .collect();
    
    println!("{:?}", scores);
}
```

## Accessing Values

```rust
use std::collections::HashMap;

fn main() {
    let mut scores = HashMap::new();
    scores.insert(String::from("Blue"), 10);
    scores.insert(String::from("Red"), 50);
    
    // Using get (returns Option)
    let team_name = String::from("Blue");
    match scores.get(&team_name) {
        Some(score) => println!("Score: {}", score),
        None => println!("Team not found"),
    }
    
    // Using get with default
    let score = scores.get(&String::from("Yellow")).unwrap_or(&0);
    println!("Yellow score: {}", score);
    
    // Direct access (panics if key doesn't exist)
    // let score = scores[&String::from("Yellow")]; // Panic!
}
```

## Iterating Over HashMaps

```rust
use std::collections::HashMap;

fn main() {
    let mut scores = HashMap::new();
    scores.insert(String::from("Blue"), 10);
    scores.insert(String::from("Red"), 50);
    
    // Iterate over key-value pairs
    for (key, value) in &scores {
        println!("{}: {}", key, value);
    }
    
    // Iterate over keys
    for key in scores.keys() {
        println!("Team: {}", key);
    }
    
    // Iterate over values
    for value in scores.values() {
        println!("Score: {}", value);
    }
    
    // Mutable iteration
    for (_, value) in &mut scores {
        *value += 10;
    }
}
```

## Updating HashMaps

### Overwriting Values
```rust
use std::collections::HashMap;

fn main() {
    let mut scores = HashMap::new();
    
    scores.insert(String::from("Blue"), 10);
    scores.insert(String::from("Blue"), 25); // Overwrites
    
    println!("{:?}", scores);
}
```

### Only Insert If Key Doesn't Exist
```rust
use std::collections::HashMap;

fn main() {
    let mut scores = HashMap::new();
    scores.insert(String::from("Blue"), 10);
    
    // entry API
    scores.entry(String::from("Yellow")).or_insert(50);
    scores.entry(String::from("Blue")).or_insert(50); // Won't overwrite
    
    println!("{:?}", scores);
}
```

### Update Based on Old Value
```rust
use std::collections::HashMap;

fn main() {
    let text = "hello world wonderful world";
    let mut word_count = HashMap::new();
    
    for word in text.split_whitespace() {
        let count = word_count.entry(word).or_insert(0);
        *count += 1;
    }
    
    println!("{:?}", word_count);
}
```

## HashMap Methods

### Useful Methods
```rust
use std::collections::HashMap;

fn main() {
    let mut map = HashMap::new();
    map.insert("a", 1);
    map.insert("b", 2);
    map.insert("c", 3);
    
    // Check if contains key
    if map.contains_key("a") {
        println!("Contains 'a'");
    }
    
    // Remove entry
    if let Some(value) = map.remove("b") {
        println!("Removed b: {}", value);
    }
    
    // Get length
    println!("Size: {}", map.len());
    
    // Check if empty
    println!("Is empty: {}", map.is_empty());
    
    // Clear all entries
    map.clear();
}
```

### Entry API
```rust
use std::collections::HashMap;

fn main() {
    let mut map = HashMap::new();
    
    // or_insert
    map.entry("a").or_insert(1);
    
    // or_insert_with
    map.entry("b").or_insert_with(|| {
        println!("Computing default value");
        2
    });
    
    // and_modify
    map.entry("a")
        .and_modify(|value| *value += 1)
        .or_insert(1);
    
    println!("{:?}", map);
}
```

## Custom Types as Keys

Keys must implement `Eq` and `Hash` traits:

```rust
use std::collections::HashMap;

#[derive(Debug, Eq, PartialEq, Hash)]
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let mut positions = HashMap::new();
    
    positions.insert(Point { x: 0, y: 0 }, "origin");
    positions.insert(Point { x: 1, y: 1 }, "diagonal");
    
    let point = Point { x: 0, y: 0 };
    if let Some(name) = positions.get(&point) {
        println!("Point name: {}", name);
    }
}
```

## HashMap with Default Values

```rust
use std::collections::HashMap;

fn main() {
    let mut map: HashMap<&str, Vec<i32>> = HashMap::new();
    
    // Add values to vectors
    map.entry("evens").or_insert(Vec::new()).push(2);
    map.entry("evens").or_insert(Vec::new()).push(4);
    map.entry("odds").or_insert(Vec::new()).push(1);
    map.entry("odds").or_insert(Vec::new()).push(3);
    
    println!("{:?}", map);
}
```

## Performance Considerations

### With Capacity
```rust
use std::collections::HashMap;

fn main() {
    // Pre-allocate capacity
    let mut map = HashMap::with_capacity(100);
    
    for i in 0..100 {
        map.insert(i, i * 2);
    }
    
    println!("Capacity: {}", map.capacity());
}
```

### Custom Hasher
```rust
use std::collections::HashMap;
use std::hash::BuildHasherDefault;
use std::collections::hash_map::DefaultHasher;

fn main() {
    // Use custom hasher for specific needs
    let mut map: HashMap<i32, &str, BuildHasherDefault<DefaultHasher>> = 
        HashMap::default();
    
    map.insert(1, "one");
    map.insert(2, "two");
}
```

## Practical Examples

### Configuration Storage
```rust
use std::collections::HashMap;

struct Config {
    settings: HashMap<String, String>,
}

impl Config {
    fn new() -> Self {
        Config {
            settings: HashMap::new(),
        }
    }
    
    fn set(&mut self, key: &str, value: &str) {
        self.settings.insert(key.to_string(), value.to_string());
    }
    
    fn get(&self, key: &str) -> Option<&String> {
        self.settings.get(key)
    }
    
    fn get_or_default(&self, key: &str, default: &str) -> String {
        self.settings.get(key)
            .cloned()
            .unwrap_or_else(|| default.to_string())
    }
}

fn main() {
    let mut config = Config::new();
    config.set("host", "localhost");
    config.set("port", "8080");
    
    println!("Host: {}", config.get_or_default("host", "127.0.0.1"));
    println!("Timeout: {}", config.get_or_default("timeout", "30"));
}
```

### Grouping Data
```rust
use std::collections::HashMap;

#[derive(Debug)]
struct Person {
    name: String,
    age: u32,
    city: String,
}

fn group_by_city(people: Vec<Person>) -> HashMap<String, Vec<Person>> {
    let mut groups = HashMap::new();
    
    for person in people {
        groups.entry(person.city.clone())
            .or_insert(Vec::new())
            .push(person);
    }
    
    groups
}

fn main() {
    let people = vec![
        Person { name: "Alice".to_string(), age: 30, city: "NYC".to_string() },
        Person { name: "Bob".to_string(), age: 25, city: "LA".to_string() },
        Person { name: "Charlie".to_string(), age: 35, city: "NYC".to_string() },
    ];
    
    let grouped = group_by_city(people);
    
    for (city, people) in grouped {
        println!("{}: {} people", city, people.len());
    }
}
```

### Caching/Memoization
```rust
use std::collections::HashMap;

struct FibCache {
    cache: HashMap<u32, u64>,
}

impl FibCache {
    fn new() -> Self {
        FibCache {
            cache: HashMap::new(),
        }
    }
    
    fn fibonacci(&mut self, n: u32) -> u64 {
        if let Some(&result) = self.cache.get(&n) {
            return result;
        }
        
        let result = match n {
            0 => 0,
            1 => 1,
            _ => self.fibonacci(n - 1) + self.fibonacci(n - 2),
        };
        
        self.cache.insert(n, result);
        result
    }
}

fn main() {
    let mut fib = FibCache::new();
    
    for i in 0..=10 {
        println!("fib({}) = {}", i, fib.fibonacci(i));
    }
}
```

## Best Practices

1. **Use `entry` API** for conditional insertions and updates
2. **Pre-allocate capacity** when size is known
3. **Consider `&str` keys** when possible to avoid allocations
4. **Use `get` instead of indexing** to handle missing keys gracefully
5. **Clone keys sparingly** - use references when possible
6. **Consider `BTreeMap`** if you need sorted keys
7. **Implement `Hash` and `Eq` correctly** for custom key types
8. **Use type aliases** for complex HashMap types