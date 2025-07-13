# Hello World in Rust

Let's write your first Rust program - the traditional "Hello, World!" example.

## Creating Your First Program

### Method 1: Using rustc directly

Create a file named `hello.rs`:

```rust
fn main() {
    println!("Hello, World!");
}
```

Compile and run the program:
```bash
rustc hello.rs
./hello
```

### Method 2: Using Cargo (Recommended)

Cargo is Rust's build system and package manager. It's the preferred way to manage Rust projects.

Create a new project:
```bash
cargo new hello_world
cd hello_world
```

This creates a project structure:
```
hello_world/
├── Cargo.toml
└── src/
    └── main.rs
```

The `src/main.rs` file already contains a Hello World program:

```rust
fn main() {
    println!("Hello, world!");
}
```

Run the program:
```bash
cargo run
```

## Understanding the Code

Let's break down the Hello World program:

```rust
fn main() {
    println!("Hello, World!");
}
```

### 1. `fn main()`
- `fn` keyword declares a function
- `main` is the entry point of every Rust program
- `()` indicates the function takes no parameters
- The function body is enclosed in curly braces `{}`

### 2. `println!`
- `println!` is a macro (indicated by the `!`)
- It prints text to the console with a newline
- Macros are similar to functions but are expanded at compile time

## Variations

### Print without newline:
```rust
fn main() {
    print!("Hello, ");
    print!("World!");
}
```

### Print with formatting:
```rust
fn main() {
    let name = "Rust";
    println!("Hello, {}!", name);
}
```

### Print multiple values:
```rust
fn main() {
    let lang = "Rust";
    let year = 2024;
    println!("Learning {} in {}", lang, year);
}
```

## Common Macros for Output

### `println!` - Print with newline
```rust
println!("This adds a newline");
```

### `print!` - Print without newline
```rust
print!("No newline");
```

### `eprintln!` - Print to standard error
```rust
eprintln!("Error: Something went wrong!");
```

### `dbg!` - Debug print
```rust
let x = 5;
dbg!(x); // Prints: [src/main.rs:2] x = 5
```

## Cargo Commands

When using Cargo, here are useful commands:

- `cargo new project_name` - Create a new project
- `cargo build` - Compile the project
- `cargo run` - Compile and run
- `cargo check` - Check for errors without building
- `cargo build --release` - Build optimized version

## Exercise

Try modifying the Hello World program to:
1. Print your name
2. Print on multiple lines
3. Use variables in the output

Example solution:
```rust
fn main() {
    let name = "Alice";
    let age = 25;
    
    println!("Hello, my name is {}", name);
    println!("I am {} years old", age);
    println!("Nice to meet you!");
}
```