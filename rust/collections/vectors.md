# Vectors in Rust

Vectors (`Vec<T>`) are resizable arrays that store values of the same type in contiguous memory.

## Creating Vectors

### Using `vec!` Macro
```rust
fn main() {
    // Create with initial values
    let v = vec![1, 2, 3, 4, 5];
    println!("Vector: {:?}", v);
    
    // Create with repeated value
    let zeros = vec![0; 5]; // [0, 0, 0, 0, 0]
    println!("Zeros: {:?}", zeros);
}
```

### Using `Vec::new()`
```rust
fn main() {
    let mut v: Vec<i32> = Vec::new();
    v.push(1);
    v.push(2);
    v.push(3);
    
    println!("Vector: {:?}", v);
}
```

### With Capacity
```rust
fn main() {
    // Pre-allocate capacity for performance
    let mut v = Vec::with_capacity(10);
    
    for i in 0..10 {
        v.push(i);
    }
    
    println!("Length: {}, Capacity: {}", v.len(), v.capacity());
}
```

## Accessing Elements

### Using Index
```rust
fn main() {
    let v = vec![1, 2, 3, 4, 5];
    
    // Direct indexing (can panic)
    let third = v[2];
    println!("Third element: {}", third);
    
    // Using get() method (returns Option)
    match v.get(2) {
        Some(third) => println!("Third element: {}", third),
        None => println!("No third element"),
    }
}
```

### Iterating
```rust
fn main() {
    let v = vec![100, 32, 57];
    
    // Immutable iteration
    for i in &v {
        println!("{}", i);
    }
    
    // Mutable iteration
    let mut v = vec![100, 32, 57];
    for i in &mut v {
        *i += 50;
    }
    println!("After mutation: {:?}", v);
}
```

## Modifying Vectors

### Adding Elements
```rust
fn main() {
    let mut v = vec![1, 2];
    
    // Add to end
    v.push(3);
    v.push(4);
    
    // Insert at specific position
    v.insert(1, 10); // Insert 10 at index 1
    
    println!("Vector: {:?}", v); // [1, 10, 2, 3, 4]
}
```

### Removing Elements
```rust
fn main() {
    let mut v = vec![1, 2, 3, 4, 5];
    
    // Remove last element
    let last = v.pop(); // Returns Option<T>
    println!("Popped: {:?}", last);
    
    // Remove at specific index
    let removed = v.remove(1); // Removes and returns element
    println!("Removed: {}", removed);
    
    println!("Vector: {:?}", v);
}
```

### Other Modifications
```rust
fn main() {
    let mut v = vec![1, 2, 3, 4, 5];
    
    // Clear all elements
    v.clear();
    println!("After clear: {:?}", v);
    
    // Extend with another collection
    v.extend([1, 2, 3]);
    v.extend_from_slice(&[4, 5, 6]);
    println!("After extend: {:?}", v);
    
    // Retain elements matching condition
    v.retain(|&x| x % 2 == 0);
    println!("Even numbers: {:?}", v);
}
```

## Slicing Vectors

```rust
fn main() {
    let v = vec![1, 2, 3, 4, 5];
    
    // Get slice
    let slice = &v[1..4];
    println!("Slice: {:?}", slice);
    
    // Mutable slice
    let mut v = vec![1, 2, 3, 4, 5];
    let slice = &mut v[1..4];
    slice[0] = 10;
    println!("Modified vector: {:?}", v);
}
```

## Vector Methods

### Searching
```rust
fn main() {
    let v = vec![1, 2, 3, 4, 5];
    
    // Check if contains
    if v.contains(&3) {
        println!("Vector contains 3");
    }
    
    // Find position
    if let Some(pos) = v.iter().position(|&x| x == 3) {
        println!("3 is at position: {}", pos);
    }
    
    // Binary search (requires sorted vector)
    let sorted = vec![1, 3, 5, 7, 9];
    match sorted.binary_search(&5) {
        Ok(pos) => println!("Found at position: {}", pos),
        Err(pos) => println!("Not found, would be at: {}", pos),
    }
}
```

### Sorting
```rust
fn main() {
    let mut v = vec![3, 1, 4, 1, 5, 9];
    
    // Sort in place
    v.sort();
    println!("Sorted: {:?}", v);
    
    // Sort with custom comparator
    let mut people = vec![("Alice", 30), ("Bob", 25), ("Charlie", 35)];
    people.sort_by(|a, b| a.1.cmp(&b.1)); // Sort by age
    println!("Sorted by age: {:?}", people);
    
    // Sort by key
    people.sort_by_key(|person| person.0); // Sort by name
    println!("Sorted by name: {:?}", people);
}
```

## Vector Patterns

### Collecting from Iterator
```rust
fn main() {
    // From range
    let v: Vec<i32> = (1..=5).collect();
    println!("From range: {:?}", v);
    
    // From map
    let doubled: Vec<i32> = v.iter().map(|x| x * 2).collect();
    println!("Doubled: {:?}", doubled);
    
    // Filter and collect
    let evens: Vec<i32> = (1..=10).filter(|x| x % 2 == 0).collect();
    println!("Evens: {:?}", evens);
}
```

### Deduplication
```rust
fn main() {
    let mut v = vec![1, 2, 2, 3, 3, 3, 4];
    v.dedup(); // Removes consecutive duplicates
    println!("Deduped: {:?}", v);
    
    // Remove all duplicates
    let mut v = vec![3, 1, 2, 3, 2, 1];
    v.sort();
    v.dedup();
    println!("Unique elements: {:?}", v);
}
```

### Chunking
```rust
fn main() {
    let v = vec![1, 2, 3, 4, 5, 6, 7, 8];
    
    // Fixed-size chunks
    for chunk in v.chunks(3) {
        println!("Chunk: {:?}", chunk);
    }
    
    // Windows (overlapping)
    for window in v.windows(3) {
        println!("Window: {:?}", window);
    }
}
```

## Memory and Performance

### Capacity Management
```rust
fn main() {
    let mut v = Vec::with_capacity(10);
    println!("Initial capacity: {}", v.capacity());
    
    for i in 0..20 {
        v.push(i);
        if v.len() == v.capacity() {
            println!("Capacity increased to: {}", v.capacity());
        }
    }
    
    // Shrink to fit
    v.shrink_to_fit();
    println!("After shrink: capacity = {}", v.capacity());
}
```

### Moving Out of Vector
```rust
fn main() {
    let v = vec![String::from("Hello"), String::from("World")];
    
    // Move out using into_iter()
    for s in v.into_iter() {
        println!("Owned: {}", s);
    }
    // v is no longer accessible
    
    // Or drain elements
    let mut v = vec![1, 2, 3, 4, 5];
    let drained: Vec<i32> = v.drain(1..4).collect();
    println!("Drained: {:?}", drained);
    println!("Remaining: {:?}", v);
}
```

## Practical Examples

### Stack Implementation
```rust
struct Stack<T> {
    items: Vec<T>,
}

impl<T> Stack<T> {
    fn new() -> Self {
        Stack { items: Vec::new() }
    }
    
    fn push(&mut self, item: T) {
        self.items.push(item);
    }
    
    fn pop(&mut self) -> Option<T> {
        self.items.pop()
    }
    
    fn peek(&self) -> Option<&T> {
        self.items.last()
    }
    
    fn is_empty(&self) -> bool {
        self.items.is_empty()
    }
}

fn main() {
    let mut stack = Stack::new();
    stack.push(1);
    stack.push(2);
    stack.push(3);
    
    while let Some(value) = stack.pop() {
        println!("Popped: {}", value);
    }
}
```

### Matrix as Vector of Vectors
```rust
fn main() {
    let rows = 3;
    let cols = 3;
    
    // Create matrix
    let mut matrix = vec![vec![0; cols]; rows];
    
    // Fill matrix
    for i in 0..rows {
        for j in 0..cols {
            matrix[i][j] = i * cols + j;
        }
    }
    
    // Print matrix
    for row in &matrix {
        println!("{:?}", row);
    }
}
```

## Best Practices

1. **Use `Vec::with_capacity`** when you know the approximate size
2. **Prefer iterators** over indexing for better safety and performance
3. **Use `get()` method** instead of indexing when element might not exist
4. **Consider using slices** (`&[T]`) in function parameters for flexibility
5. **Use `drain()` to remove and use elements** efficiently
6. **Sort before dedup** for removing all duplicates
7. **Be aware of reallocation costs** when growing vectors