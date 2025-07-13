# Cargo Basics

Cargo is Rust's build system and package manager. It handles downloading dependencies, compiling packages, making distributable packages, and uploading them to crates.io.

## Creating a New Project

### Binary Project
```bash
cargo new my_project
cd my_project
```

This creates:
```
my_project/
├── Cargo.toml
└── src/
    └── main.rs
```

### Library Project
```bash
cargo new my_library --lib
```

This creates:
```
my_library/
├── Cargo.toml
└── src/
    └── lib.rs
```

## Understanding Cargo.toml

The manifest file that contains metadata about your project:

```toml
[package]
name = "my_project"
version = "0.1.0"
edition = "2021"
authors = ["Your Name <you@example.com>"]
description = "A brief description"
license = "MIT"

[dependencies]
serde = "1.0"
tokio = { version = "1", features = ["full"] }

[dev-dependencies]
assert_cmd = "2.0"

[build-dependencies]
cc = "1.0"
```

## Basic Cargo Commands

### Building and Running
```bash
# Build the project
cargo build

# Build in release mode (optimized)
cargo build --release

# Run the project
cargo run

# Run with arguments
cargo run -- arg1 arg2

# Run in release mode
cargo run --release
```

### Checking and Testing
```bash
# Check if code compiles (faster than build)
cargo check

# Run tests
cargo test

# Run specific test
cargo test test_name

# Run tests with output
cargo test -- --nocapture

# Run benchmarks
cargo bench
```

### Documentation
```bash
# Generate documentation
cargo doc

# Generate and open documentation
cargo doc --open

# Include dependencies documentation
cargo doc --no-deps
```

## Managing Dependencies

### Adding Dependencies

Add to `Cargo.toml`:
```toml
[dependencies]
rand = "0.8"
serde = { version = "1.0", features = ["derive"] }
my_crate = { path = "../my_crate" }
my_git_crate = { git = "https://github.com/user/repo.git" }
```

### Using Dependencies
```rust
use rand::Rng;
use serde::{Serialize, Deserialize};

#[derive(Serialize, Deserialize)]
struct Config {
    name: String,
    value: i32,
}

fn main() {
    let mut rng = rand::thread_rng();
    let random: i32 = rng.gen_range(1..=100);
    println!("Random number: {}", random);
}
```

### Updating Dependencies
```bash
# Update dependencies to latest compatible versions
cargo update

# Update specific dependency
cargo update -p rand

# Generate Cargo.lock
cargo generate-lockfile
```

## Cargo Workspaces

For managing multiple related packages:

### Workspace Cargo.toml
```toml
[workspace]
members = [
    "package1",
    "package2",
    "shared",
]

[workspace.dependencies]
serde = "1.0"
tokio = "1"
```

### Member Package Cargo.toml
```toml
[package]
name = "package1"
version = "0.1.0"
edition = "2021"

[dependencies]
serde = { workspace = true }
shared = { path = "../shared" }
```

## Features

Conditional compilation and optional dependencies:

```toml
[features]
default = ["json"]
json = ["serde", "serde_json"]
async = ["tokio"]
full = ["json", "async"]

[dependencies]
serde = { version = "1.0", optional = true }
serde_json = { version = "1.0", optional = true }
tokio = { version = "1", optional = true }
```

Using features in code:
```rust
#[cfg(feature = "json")]
pub fn parse_json(data: &str) -> Result<Value, Error> {
    serde_json::from_str(data)
}

#[cfg(feature = "async")]
pub async fn fetch_data(url: &str) -> Result<String, Error> {
    // Async implementation
}
```

## Build Scripts

Custom build logic with `build.rs`:

```rust
// build.rs
fn main() {
    // Generate code
    println!("cargo:rerun-if-changed=build.rs");
    
    // Compile C code
    cc::Build::new()
        .file("src/helper.c")
        .compile("helper");
    
    // Set environment variables
    println!("cargo:rustc-env=BUILD_TIME={}", chrono::Utc::now());
}
```

## Publishing to crates.io

### Preparing for Publication
```toml
[package]
name = "unique-crate-name"
version = "0.1.0"
authors = ["Your Name <you@example.com>"]
edition = "2021"
description = "A useful crate"
readme = "README.md"
homepage = "https://example.com"
repository = "https://github.com/you/repo"
license = "MIT OR Apache-2.0"
keywords = ["utility", "helper"]
categories = ["development-tools"]
```

### Publishing Commands
```bash
# Check package
cargo package --list

# Create .crate file
cargo package

# Publish (requires crates.io account)
cargo publish

# Publish dry run
cargo publish --dry-run
```

## Cargo Configuration

### .cargo/config.toml
```toml
[build]
target-dir = "target"
incremental = true

[target.x86_64-pc-windows-msvc]
linker = "rust-lld.exe"

[alias]
b = "build"
r = "run"
t = "test"
br = "build --release"

[env]
RUST_LOG = "debug"
```

## Practical Examples

### Multi-Binary Project
```toml
[[bin]]
name = "server"
path = "src/bin/server.rs"

[[bin]]
name = "client"
path = "src/bin/client.rs"
```

Run specific binary:
```bash
cargo run --bin server
cargo run --bin client
```

### Example Tests
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
}

// examples/demo.rs
fn main() {
    println!("2 + 2 = {}", my_crate::add(2, 2));
}
```

Run example:
```bash
cargo run --example demo
```

### Integration Tests
```rust
// tests/integration_test.rs
use my_crate;

#[test]
fn test_integration() {
    assert_eq!(my_crate::add(5, 5), 10);
}
```

## Common Cargo Extensions

```bash
# Install cargo extension
cargo install cargo-watch cargo-edit cargo-audit

# Watch for changes and rebuild
cargo watch -x build

# Add dependency from command line
cargo add serde

# Check for security vulnerabilities
cargo audit
```

## Best Practices

1. **Keep Cargo.toml organized** with clear sections
2. **Use workspace** for multi-package projects
3. **Pin versions** in libraries, use ranges in applications
4. **Include Cargo.lock** in version control for binaries
5. **Use features** for optional functionality
6. **Write examples** in the examples/ directory
7. **Document with cargo doc** comments
8. **Use cargo fmt** and **cargo clippy** for code quality
9. **Set up CI** to run cargo test and cargo clippy
10. **Use semantic versioning** for releases