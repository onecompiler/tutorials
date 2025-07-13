# Structs in Rust

Structs are custom data types that let you name and package together multiple related values.

## Defining Structs

Basic struct definition:

```rust
struct User {
    username: String,
    email: String,
    active: bool,
    sign_in_count: u64,
}

fn main() {
    let user = User {
        email: String::from("user@example.com"),
        username: String::from("someuser"),
        active: true,
        sign_in_count: 1,
    };
    
    println!("User: {}", user.username);
}
```

## Creating Instances

### Using Field Init Shorthand
```rust
fn build_user(email: String, username: String) -> User {
    User {
        email,      // Shorthand for email: email
        username,   // Shorthand for username: username
        active: true,
        sign_in_count: 1,
    }
}
```

### Struct Update Syntax
```rust
fn main() {
    let user1 = User {
        email: String::from("user@example.com"),
        username: String::from("someuser"),
        active: true,
        sign_in_count: 1,
    };
    
    let user2 = User {
        email: String::from("another@example.com"),
        ..user1  // Use remaining fields from user1
    };
}
```

## Tuple Structs

Structs without named fields:

```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

fn main() {
    let black = Color(0, 0, 0);
    let origin = Point(0, 0, 0);
    
    println!("Color: ({}, {}, {})", black.0, black.1, black.2);
}
```

## Unit-Like Structs

Structs without any fields:

```rust
struct AlwaysEqual;

fn main() {
    let subject = AlwaysEqual;
    // Useful for implementing traits
}
```

## Methods

Define methods using `impl` blocks:

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
    
    fn can_hold(&self, other: &Rectangle) -> bool {
        self.width > other.width && self.height > other.height
    }
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };
    
    println!("Area: {}", rect1.area());
}
```

## Associated Functions

Functions that don't take `self`:

```rust
impl Rectangle {
    fn square(size: u32) -> Rectangle {
        Rectangle {
            width: size,
            height: size,
        }
    }
}

fn main() {
    let sq = Rectangle::square(3);
    println!("Square: {:?}", sq);
}
```

## Multiple `impl` Blocks

You can have multiple `impl` blocks:

```rust
impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

impl Rectangle {
    fn perimeter(&self) -> u32 {
        2 * (self.width + self.height)
    }
}
```

## Derived Traits

Common traits can be automatically implemented:

```rust
#[derive(Debug, Clone, PartialEq)]
struct Point {
    x: f32,
    y: f32,
}

fn main() {
    let p1 = Point { x: 1.0, y: 2.0 };
    let p2 = p1.clone();
    
    println!("p1: {:?}", p1);  // Debug trait
    println!("Equal: {}", p1 == p2);  // PartialEq trait
}
```

## Generic Structs

Structs with type parameters:

```rust
struct Point<T> {
    x: T,
    y: T,
}

impl<T> Point<T> {
    fn x(&self) -> &T {
        &self.x
    }
}

fn main() {
    let integer = Point { x: 5, y: 10 };
    let float = Point { x: 1.0, y: 4.0 };
    
    println!("Integer x: {}", integer.x());
    println!("Float x: {}", float.x());
}
```

### Multiple Type Parameters
```rust
struct Point<T, U> {
    x: T,
    y: U,
}

fn main() {
    let mixed = Point { x: 5, y: 4.0 };
    println!("x: {}, y: {}", mixed.x, mixed.y);
}
```

## Struct Visibility

Control field visibility:

```rust
mod my_module {
    pub struct PublicStruct {
        pub public_field: i32,
        private_field: i32,
    }
    
    impl PublicStruct {
        pub fn new() -> Self {
            PublicStruct {
                public_field: 1,
                private_field: 2,
            }
        }
    }
}
```

## Builder Pattern

Common pattern for complex structs:

```rust
#[derive(Debug)]
struct Server {
    host: String,
    port: u16,
    timeout: u64,
    max_connections: u32,
}

struct ServerBuilder {
    host: String,
    port: u16,
    timeout: u64,
    max_connections: u32,
}

impl ServerBuilder {
    fn new(host: String) -> Self {
        ServerBuilder {
            host,
            port: 8080,
            timeout: 30,
            max_connections: 100,
        }
    }
    
    fn port(mut self, port: u16) -> Self {
        self.port = port;
        self
    }
    
    fn timeout(mut self, timeout: u64) -> Self {
        self.timeout = timeout;
        self
    }
    
    fn build(self) -> Server {
        Server {
            host: self.host,
            port: self.port,
            timeout: self.timeout,
            max_connections: self.max_connections,
        }
    }
}

fn main() {
    let server = ServerBuilder::new(String::from("localhost"))
        .port(3000)
        .timeout(60)
        .build();
    
    println!("Server: {:?}", server);
}
```

## Practical Examples

### Game Entity
```rust
#[derive(Debug)]
struct Player {
    name: String,
    health: i32,
    mana: i32,
    level: u32,
}

impl Player {
    fn new(name: String) -> Self {
        Player {
            name,
            health: 100,
            mana: 50,
            level: 1,
        }
    }
    
    fn take_damage(&mut self, damage: i32) {
        self.health -= damage;
        if self.health < 0 {
            self.health = 0;
        }
    }
    
    fn heal(&mut self, amount: i32) {
        self.health += amount;
        if self.health > 100 {
            self.health = 100;
        }
    }
    
    fn is_alive(&self) -> bool {
        self.health > 0
    }
}

fn main() {
    let mut player = Player::new(String::from("Hero"));
    
    player.take_damage(30);
    println!("After damage: {:?}", player);
    
    player.heal(20);
    println!("After heal: {:?}", player);
}
```

### Configuration Struct
```rust
use std::fs;

#[derive(Debug)]
struct Config {
    database_url: String,
    port: u16,
    debug_mode: bool,
}

impl Config {
    fn from_file(path: &str) -> Result<Self, std::io::Error> {
        let contents = fs::read_to_string(path)?;
        // Parse contents (simplified)
        Ok(Config {
            database_url: String::from("localhost:5432"),
            port: 8080,
            debug_mode: true,
        })
    }
    
    fn default() -> Self {
        Config {
            database_url: String::from("localhost:5432"),
            port: 3000,
            debug_mode: false,
        }
    }
}

fn main() {
    let config = Config::default();
    println!("Config: {:?}", config);
}
```

## Best Practices

1. **Use meaningful field names** that clearly describe the data
2. **Implement `Debug` trait** for easier debugging
3. **Consider implementing `Default`** for structs with sensible defaults
4. **Use builder pattern** for structs with many optional fields
5. **Keep structs focused** - one struct should represent one concept
6. **Use tuple structs** for simple wrappers around values
7. **Document public structs and methods** with doc comments