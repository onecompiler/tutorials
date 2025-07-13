# Loops in Rust

Rust provides several ways to execute code repeatedly. Let's explore each type of loop and when to use them.

## The `loop` Expression

The `loop` keyword creates an infinite loop that must be explicitly broken:

```rust
fn main() {
    let mut counter = 0;
    
    loop {
        counter += 1;
        println!("Counter: {}", counter);
        
        if counter == 5 {
            break;
        }
    }
}
```

### Returning Values from `loop`
```rust
fn main() {
    let mut counter = 0;
    
    let result = loop {
        counter += 1;
        
        if counter == 10 {
            break counter * 2;
        }
    };
    
    println!("Result: {}", result); // 20
}
```

### Loop Labels
```rust
fn main() {
    let mut count = 0;
    
    'outer: loop {
        println!("count = {}", count);
        let mut remaining = 10;
        
        loop {
            println!("remaining = {}", remaining);
            if remaining == 9 {
                break;
            }
            if count == 2 {
                break 'outer;
            }
            remaining -= 1;
        }
        
        count += 1;
    }
    println!("End count = {}", count);
}
```

## The `while` Loop

Execute code while a condition is true:

```rust
fn main() {
    let mut number = 3;
    
    while number != 0 {
        println!("{}!", number);
        number -= 1;
    }
    
    println!("LIFTOFF!");
}
```

### While with Complex Conditions
```rust
fn main() {
    let mut x = 5;
    let mut y = 10;
    
    while x < 10 && y > 0 {
        println!("x: {}, y: {}", x, y);
        x += 1;
        y -= 2;
    }
}
```

## The `for` Loop

The most common way to loop in Rust:

### Iterating Over Collections
```rust
fn main() {
    let numbers = [10, 20, 30, 40, 50];
    
    for element in numbers {
        println!("Value: {}", element);
    }
}
```

### Using Ranges
```rust
fn main() {
    // Exclusive range (1 to 4)
    for number in 1..5 {
        println!("{}", number);
    }
    
    // Inclusive range (1 to 5)
    for number in 1..=5 {
        println!("{}", number);
    }
    
    // Reverse range
    for number in (1..5).rev() {
        println!("{}", number);
    }
}
```

### Enumerate for Index and Value
```rust
fn main() {
    let fruits = ["apple", "banana", "orange"];
    
    for (index, fruit) in fruits.iter().enumerate() {
        println!("{}: {}", index, fruit);
    }
}
```

## Loop Control

### `break` Statement
```rust
fn main() {
    for i in 0..10 {
        if i == 5 {
            break; // Exit the loop
        }
        println!("{}", i);
    }
}
```

### `continue` Statement
```rust
fn main() {
    for i in 0..10 {
        if i % 2 == 0 {
            continue; // Skip even numbers
        }
        println!("{}", i);
    }
}
```

## Iterator Methods

Rust's iterators provide powerful methods:

### `map` and `collect`
```rust
fn main() {
    let numbers = vec![1, 2, 3, 4, 5];
    let doubled: Vec<i32> = numbers.iter().map(|x| x * 2).collect();
    
    println!("Original: {:?}", numbers);
    println!("Doubled: {:?}", doubled);
}
```

### `filter`
```rust
fn main() {
    let numbers = vec![1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
    let evens: Vec<&i32> = numbers.iter().filter(|x| *x % 2 == 0).collect();
    
    println!("Even numbers: {:?}", evens);
}
```

### `fold` (Reduce)
```rust
fn main() {
    let numbers = vec![1, 2, 3, 4, 5];
    let sum = numbers.iter().fold(0, |acc, x| acc + x);
    
    println!("Sum: {}", sum);
}
```

## Practical Examples

### Fibonacci Sequence
```rust
fn main() {
    let mut a = 0;
    let mut b = 1;
    
    println!("Fibonacci sequence:");
    for _ in 0..10 {
        println!("{}", a);
        let temp = a + b;
        a = b;
        b = temp;
    }
}
```

### Finding Prime Numbers
```rust
fn is_prime(n: u32) -> bool {
    if n < 2 {
        return false;
    }
    
    for i in 2..=(n as f64).sqrt() as u32 {
        if n % i == 0 {
            return false;
        }
    }
    
    true
}

fn main() {
    println!("Prime numbers up to 50:");
    for num in 2..=50 {
        if is_prime(num) {
            print!("{} ", num);
        }
    }
    println!();
}
```

### User Input Loop
```rust
use std::io;

fn main() {
    loop {
        println!("Enter a number (or 'quit' to exit):");
        
        let mut input = String::new();
        io::stdin().read_line(&mut input).expect("Failed to read line");
        
        let input = input.trim();
        
        if input == "quit" {
            break;
        }
        
        match input.parse::<i32>() {
            Ok(num) => println!("You entered: {}", num),
            Err(_) => println!("Invalid number!"),
        }
    }
}
```

### Nested Loops
```rust
fn main() {
    // Multiplication table
    for i in 1..=5 {
        for j in 1..=5 {
            print!("{:4}", i * j);
        }
        println!();
    }
}
```

### While Let
```rust
fn main() {
    let mut stack = vec![1, 2, 3, 4, 5];
    
    while let Some(top) = stack.pop() {
        println!("Popped: {}", top);
    }
}
```

## Performance Tips

1. Use `for` loops with iterators instead of indexing when possible
2. Consider using `iterator` methods instead of manual loops
3. Pre-allocate collections when size is known
4. Use `loop` for truly infinite loops instead of `while true`

### Example: Efficient vs Inefficient
```rust
fn main() {
    let numbers = vec![1, 2, 3, 4, 5];
    
    // Less efficient - indexing
    let mut sum1 = 0;
    for i in 0..numbers.len() {
        sum1 += numbers[i];
    }
    
    // More efficient - iterator
    let sum2: i32 = numbers.iter().sum();
    
    println!("Sum1: {}, Sum2: {}", sum1, sum2);
}
```