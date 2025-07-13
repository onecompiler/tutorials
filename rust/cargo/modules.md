# Modules and Packages in Rust

Rust's module system helps organize code into logical units for better maintainability and reusability.

## Module Basics

### Defining Modules
```rust
// In src/main.rs or src/lib.rs
mod front_of_house {
    mod hosting {
        fn add_to_waitlist() {}
        fn seat_at_table() {}
    }
    
    mod serving {
        fn take_order() {}
        fn serve_order() {}
        fn take_payment() {}
    }
}
```

### Public vs Private
```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
        
        // Private by default
        fn seat_at_table() {}
    }
}

pub fn eat_at_restaurant() {
    // Absolute path
    crate::front_of_house::hosting::add_to_waitlist();
    
    // Relative path
    front_of_house::hosting::add_to_waitlist();
    
    // This won't work - seat_at_table is private
    // front_of_house::hosting::seat_at_table();
}
```

## File Structure

### Single File Modules
```rust
// src/lib.rs
mod garden;  // Looks for src/garden.rs

// src/garden.rs
pub fn plant_vegetables() {
    println!("Planting vegetables");
}
```

### Module Directories
```rust
// src/lib.rs
mod garden;  // Looks for src/garden/mod.rs or src/garden.rs

// src/garden/mod.rs
pub mod vegetables;
pub mod fruits;

// src/garden/vegetables.rs
pub fn plant() {
    println!("Planting vegetables");
}

// src/garden/fruits.rs
pub fn plant() {
    println!("Planting fruits");
}
```

### Modern Module Structure (Rust 2018+)
```rust
// src/lib.rs
mod garden;  // Can look for src/garden.rs

// src/garden.rs
pub mod vegetables;  // Looks for src/garden/vegetables.rs
pub mod fruits;      // Looks for src/garden/fruits.rs
```

## The `use` Keyword

### Bringing Paths into Scope
```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

use crate::front_of_house::hosting;

pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
}
```

### Creating Idiomatic `use` Paths
```rust
// For functions: bring parent module into scope
use std::collections::HashMap;
let mut map = HashMap::new();

// For structs, enums, etc.: bring the item itself
use std::io::Result;
fn read_file() -> Result<String> {
    // ...
}
```

### Multiple Items
```rust
// Nested paths
use std::{cmp::Ordering, io};

// Multiple items from same module
use std::io::{self, Write};

// Glob operator (use sparingly)
use std::collections::*;
```

## Re-exporting with `pub use`

```rust
// src/lib.rs
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

// Re-export to make available at crate root
pub use crate::front_of_house::hosting;

// Users can now do:
// use restaurant::hosting;
```

## Package Structure

### Binary and Library Crates
```
my_package/
├── Cargo.toml
├── src/
│   ├── main.rs    # Binary crate root
│   ├── lib.rs     # Library crate root
│   └── bin/
│       ├── tool1.rs   # Additional binary
│       └── tool2.rs   # Additional binary
```

### Complex Project Structure
```
my_project/
├── Cargo.toml
├── src/
│   ├── lib.rs
│   ├── config/
│   │   ├── mod.rs
│   │   ├── parser.rs
│   │   └── validator.rs
│   ├── network/
│   │   ├── mod.rs
│   │   ├── client.rs
│   │   └── server.rs
│   └── utils/
│       ├── mod.rs
│       └── helpers.rs
```

## Visibility and Privacy

### Super and Self
```rust
fn function() {
    println!("called function()");
}

mod cool {
    pub fn function() {
        println!("called cool::function()");
    }
    
    pub mod nested {
        pub fn function() {
            println!("called cool::nested::function()");
            
            // Call parent module function
            super::function();
            
            // Call crate function
            crate::function();
        }
    }
}
```

### Struct Field Visibility
```rust
mod back_of_house {
    pub struct Breakfast {
        pub toast: String,
        seasonal_fruit: String,  // Private field
    }
    
    impl Breakfast {
        pub fn summer(toast: &str) -> Breakfast {
            Breakfast {
                toast: String::from(toast),
                seasonal_fruit: String::from("peaches"),
            }
        }
    }
}

pub fn eat_at_restaurant() {
    let mut meal = back_of_house::Breakfast::summer("Rye");
    meal.toast = String::from("Wheat");
    
    // This won't work - seasonal_fruit is private
    // meal.seasonal_fruit = String::from("blueberries");
}
```

## Practical Examples

### API Module Organization
```rust
// src/lib.rs
pub mod api {
    pub mod v1 {
        pub mod users;
        pub mod posts;
    }
    
    pub mod v2 {
        pub mod users;
        pub mod posts;
        pub mod comments;
    }
}

// src/api/v1/users.rs
use crate::models::User;

pub fn get_user(id: u64) -> Option<User> {
    // Implementation
}

pub fn create_user(data: UserData) -> Result<User, Error> {
    // Implementation
}
```

### Internal Module Pattern
```rust
// src/database/mod.rs
mod connection;
mod query_builder;

// Re-export public interface
pub use connection::Connection;
pub use query_builder::QueryBuilder;

// Keep implementation details private
use connection::ConnectionPool;
use query_builder::internal_helper;
```

### Test Module Pattern
```rust
// src/lib.rs
pub fn add(a: i32, b: i32) -> i32 {
    a + b
}

#[cfg(test)]
mod tests {
    use super::*;
    
    #[test]
    fn test_add() {
        assert_eq!(add(2, 2), 4);
    }
    
    mod integration {
        use super::*;
        
        #[test]
        fn test_complex_scenario() {
            // Complex test
        }
    }
}
```

### Plugin Architecture
```rust
// src/plugins/mod.rs
pub trait Plugin {
    fn name(&self) -> &str;
    fn execute(&self);
}

pub mod builtin;
pub mod custom;

// src/plugins/builtin/mod.rs
mod logger;
mod metrics;

pub use logger::LoggerPlugin;
pub use metrics::MetricsPlugin;

// src/main.rs
use my_app::plugins::{Plugin, builtin::LoggerPlugin};

fn main() {
    let plugin = LoggerPlugin::new();
    plugin.execute();
}
```

## Module Best Practices

### Clear Module Hierarchy
```rust
// Good: Clear, logical organization
crate
├── models
│   ├── user
│   ├── post
│   └── comment
├── controllers
│   ├── user_controller
│   └── post_controller
├── services
│   ├── auth_service
│   └── email_service
└── utils
    ├── validators
    └── formatters
```

### Module Documentation
```rust
//! # My Library
//! 
//! `my_library` provides utilities for working with data.

/// The authentication module handles user authentication
pub mod auth {
    //! Authentication and authorization utilities
    
    /// Authenticates a user with credentials
    pub fn authenticate(username: &str, password: &str) -> bool {
        // Implementation
    }
}
```

## Common Patterns

### Prelude Module
```rust
// src/prelude.rs
pub use crate::error::{Error, Result};
pub use crate::config::Config;
pub use crate::types::*;

// Users can import everything commonly needed:
// use my_crate::prelude::*;
```

### Re-export Pattern
```rust
// src/lib.rs
mod implementation {
    pub struct InternalThing;
    pub struct PublicThing;
}

// Selective re-export
pub use implementation::PublicThing;
// InternalThing remains private
```

## Tips and Guidelines

1. **Keep modules focused** on a single responsibility
2. **Use clear, descriptive names** for modules
3. **Minimize public exports** - only expose what's necessary
4. **Group related functionality** together
5. **Use `mod.rs` sparingly** in modern Rust (prefer named files)
6. **Document module purpose** with module-level docs
7. **Create a prelude** for commonly used items
8. **Use `pub(crate)`** for crate-internal visibility
9. **Organize tests** near the code they test
10. **Consider the user's perspective** when designing module structure