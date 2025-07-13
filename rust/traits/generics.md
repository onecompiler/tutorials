# Generics in Rust

Generics allow you to write code that works with multiple types while maintaining type safety. They enable code reuse without sacrificing performance.

## Generic Functions

### Basic Generic Function
```rust
fn largest<T: PartialOrd>(list: &[T]) -> &T {
    let mut largest = &list[0];
    
    for item in list {
        if item > largest {
            largest = item;
        }
    }
    
    largest
}

fn main() {
    let numbers = vec![34, 50, 25, 100, 65];
    let chars = vec!['y', 'm', 'a', 'q'];
    
    println!("Largest number: {}", largest(&numbers));
    println!("Largest char: {}", largest(&chars));
}
```

### Multiple Generic Types
```rust
fn swap<T, U>(pair: (T, U)) -> (U, T) {
    (pair.1, pair.0)
}

fn main() {
    let pair = (5, "hello");
    let swapped = swap(pair);
    println!("Swapped: {:?}", swapped); // ("hello", 5)
}
```

## Generic Structs

### Single Type Parameter
```rust
struct Point<T> {
    x: T,
    y: T,
}

impl<T> Point<T> {
    fn new(x: T, y: T) -> Self {
        Point { x, y }
    }
    
    fn x(&self) -> &T {
        &self.x
    }
}

fn main() {
    let integer_point = Point::new(5, 10);
    let float_point = Point::new(1.0, 4.0);
    
    println!("x: {}", integer_point.x());
}
```

### Multiple Type Parameters
```rust
struct Point<T, U> {
    x: T,
    y: U,
}

impl<T, U> Point<T, U> {
    fn mixup<V, W>(self, other: Point<V, W>) -> Point<T, W> {
        Point {
            x: self.x,
            y: other.y,
        }
    }
}

fn main() {
    let p1 = Point { x: 5, y: 10.4 };
    let p2 = Point { x: "Hello", y: 'c' };
    
    let p3 = p1.mixup(p2);
    println!("p3.x = {}, p3.y = {}", p3.x, p3.y); // 5, 'c'
}
```

## Generic Enums

### Result and Option
```rust
// Standard library definitions
enum Option<T> {
    Some(T),
    None,
}

enum Result<T, E> {
    Ok(T),
    Err(E),
}

// Custom generic enum
enum BinaryTree<T> {
    Empty,
    Node {
        value: T,
        left: Box<BinaryTree<T>>,
        right: Box<BinaryTree<T>>,
    },
}
```

## Generic Implementations

### Type-Specific Implementations
```rust
struct Container<T> {
    value: T,
}

// Generic implementation
impl<T> Container<T> {
    fn new(value: T) -> Self {
        Container { value }
    }
}

// Implementation only for specific type
impl Container<f32> {
    fn distance_from_origin(&self) -> f32 {
        self.value.abs()
    }
}

fn main() {
    let c1 = Container::new(5);
    let c2 = Container::new(3.0f32);
    
    // c1.distance_from_origin(); // Error: not available for i32
    println!("Distance: {}", c2.distance_from_origin());
}
```

## Trait Bounds

### Basic Bounds
```rust
use std::fmt::Display;

fn print_it<T: Display>(item: T) {
    println!("Value: {}", item);
}

// Multiple bounds
fn compare_and_display<T: Display + PartialOrd>(a: T, b: T) {
    if a > b {
        println!("{} is greater than {}", a, b);
    } else {
        println!("{} is less than or equal to {}", a, b);
    }
}
```

### Where Clauses
```rust
use std::fmt::Debug;

fn some_function<T, U>(t: &T, u: &U) -> i32
where
    T: Display + Clone,
    U: Clone + Debug,
{
    // Function body
    42
}

// More readable for complex bounds
struct Wrapper<T>
where
    T: Display + Clone + Debug,
{
    value: T,
}
```

## Associated Types vs Generics

### Using Generics
```rust
trait Container<T> {
    fn add(&mut self, item: T);
    fn get(&self) -> Option<&T>;
}

struct MyContainer<T> {
    item: Option<T>,
}

impl<T> Container<T> for MyContainer<T> {
    fn add(&mut self, item: T) {
        self.item = Some(item);
    }
    
    fn get(&self) -> Option<&T> {
        self.item.as_ref()
    }
}
```

### Using Associated Types
```rust
trait Container {
    type Item;
    
    fn add(&mut self, item: Self::Item);
    fn get(&self) -> Option<&Self::Item>;
}

struct MyContainer<T> {
    item: Option<T>,
}

impl<T> Container for MyContainer<T> {
    type Item = T;
    
    fn add(&mut self, item: Self::Item) {
        self.item = Some(item);
    }
    
    fn get(&self) -> Option<&Self::Item> {
        self.item.as_ref()
    }
}
```

## Const Generics

```rust
struct ArrayWrapper<T, const N: usize> {
    data: [T; N],
}

impl<T: Default + Copy, const N: usize> ArrayWrapper<T, N> {
    fn new() -> Self {
        ArrayWrapper {
            data: [T::default(); N],
        }
    }
}

fn main() {
    let arr: ArrayWrapper<i32, 5> = ArrayWrapper::new();
    let arr2: ArrayWrapper<f64, 10> = ArrayWrapper::new();
}
```

## Practical Examples

### Generic Cache
```rust
use std::collections::HashMap;
use std::hash::Hash;

struct Cache<K, V> {
    storage: HashMap<K, V>,
    capacity: usize,
}

impl<K: Eq + Hash, V> Cache<K, V> {
    fn new(capacity: usize) -> Self {
        Cache {
            storage: HashMap::new(),
            capacity,
        }
    }
    
    fn get(&self, key: &K) -> Option<&V> {
        self.storage.get(key)
    }
    
    fn insert(&mut self, key: K, value: V) {
        if self.storage.len() >= self.capacity {
            // Simple eviction: remove first item
            if let Some(first_key) = self.storage.keys().next().cloned() {
                self.storage.remove(&first_key);
            }
        }
        self.storage.insert(key, value);
    }
}

fn main() {
    let mut cache = Cache::new(3);
    cache.insert("key1", "value1");
    cache.insert("key2", 42);
    
    // Different cache with different types
    let mut number_cache: Cache<i32, String> = Cache::new(5);
    number_cache.insert(1, String::from("one"));
}
```

### Generic Queue
```rust
struct Queue<T> {
    items: Vec<T>,
}

impl<T> Queue<T> {
    fn new() -> Self {
        Queue { items: Vec::new() }
    }
    
    fn enqueue(&mut self, item: T) {
        self.items.push(item);
    }
    
    fn dequeue(&mut self) -> Option<T> {
        if self.items.is_empty() {
            None
        } else {
            Some(self.items.remove(0))
        }
    }
    
    fn is_empty(&self) -> bool {
        self.items.is_empty()
    }
}

fn main() {
    let mut q = Queue::new();
    q.enqueue(1);
    q.enqueue(2);
    q.enqueue(3);
    
    while let Some(item) = q.dequeue() {
        println!("Dequeued: {}", item);
    }
}
```

### Generic Result Type
```rust
#[derive(Debug)]
enum MyResult<T, E> {
    Success(T),
    Failure(E),
}

impl<T, E> MyResult<T, E> {
    fn is_success(&self) -> bool {
        matches!(self, MyResult::Success(_))
    }
    
    fn map<U, F>(self, f: F) -> MyResult<U, E>
    where
        F: FnOnce(T) -> U,
    {
        match self {
            MyResult::Success(value) => MyResult::Success(f(value)),
            MyResult::Failure(e) => MyResult::Failure(e),
        }
    }
}

fn divide(a: f64, b: f64) -> MyResult<f64, String> {
    if b == 0.0 {
        MyResult::Failure(String::from("Division by zero"))
    } else {
        MyResult::Success(a / b)
    }
}

fn main() {
    let result = divide(10.0, 2.0)
        .map(|x| x * 2.0);
    
    println!("Result: {:?}", result);
}
```

## Performance

Generics in Rust have zero-cost abstraction through monomorphization:

```rust
// This generic function...
fn identity<T>(x: T) -> T {
    x
}

// When called with i32 and String...
fn main() {
    identity(5);
    identity(String::from("hello"));
}

// Compiler generates these specific versions:
// fn identity_i32(x: i32) -> i32 { x }
// fn identity_String(x: String) -> String { x }
```

## Best Practices

1. **Use descriptive type parameter names** (T for single type, K/V for key/value)
2. **Minimize trait bounds** - only require what you need
3. **Consider using where clauses** for complex bounds
4. **Use associated types** when there's only one logical type per implementation
5. **Leverage type inference** when possible
6. **Document generic parameters** and their requirements
7. **Consider const generics** for array sizes
8. **Be aware of monomorphization** impact on binary size