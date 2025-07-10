# Introduction to Scala

Scala is a modern multi-paradigm programming language designed by Martin Odersky at EPFL. First released in 2003, Scala combines object-oriented and functional programming in one concise, high-level language. The name Scala comes from "scalable language," reflecting its design to grow with user needs.

## Why Scala?

Scala runs on the Java Virtual Machine (JVM) and is fully interoperable with Java, allowing you to use existing Java libraries while enjoying Scala's more expressive syntax and powerful features.

## Key Features

* **Statically Typed**: Strong type system with type inference
* **Multi-Paradigm**: Seamlessly combines object-oriented and functional programming
* **Concise**: Expressive syntax reduces boilerplate code
* **Immutability by Default**: Encourages functional programming practices
* **Pattern Matching**: Powerful feature for working with data structures
* **Traits**: Flexible mechanism for composition and inheritance
* **Actor Model**: Built-in support for concurrent and distributed systems
* **Type Inference**: Compiler infers types, reducing verbosity
* **Higher-Order Functions**: Functions are first-class citizens
* **Lazy Evaluation**: Evaluates expressions only when needed
* **Rich Collections**: Immutable and mutable collection libraries
* **JVM Interoperability**: Use any Java library seamlessly

## Installation

### Prerequisites
- Java 8 or higher (Java 11 or 17 recommended)
- Set JAVA_HOME environment variable

### Using Scala CLI (Recommended)

Scala CLI is the modern way to get started with Scala:

```bash
# macOS with Homebrew
brew install scala-cli

# Windows with Scoop
scoop install scala-cli

# Linux/macOS/Windows
curl -sSLf https://scala-cli.virtuslab.org/get | sh
```

### Using SBT (Scala Build Tool)

```bash
# macOS with Homebrew
brew install sbt

# Windows - Download installer from scala-sbt.org
# Linux
echo "deb https://repo.scala-sbt.org/scalasbt/debian all main" | sudo tee /etc/apt/sources.list.d/sbt.list
curl -sL "https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x99E82A75642AC823" | sudo apt-key add
sudo apt-get update
sudo apt-get install sbt
```

### IDE Support

1. **IntelliJ IDEA** with Scala plugin (recommended)
2. **Visual Studio Code** with Metals extension
3. **Eclipse** with Scala IDE plugin

### Verify Installation

```bash
# Using Scala CLI
scala-cli --version

# Using SBT
sbt --version

# Start Scala REPL
scala-cli repl
# or
sbt console
```

## Using OneCompiler

For quick experimentation without installation:
1. Visit [OneCompiler Scala](https://onecompiler.com/scala)
2. Write and run Scala code instantly
3. Perfect for learning and testing

## Your First Scala Program

### Hello World

```scala
// HelloWorld.scala
object HelloWorld {
  def main(args: Array[String]): Unit = {
    println("Hello, World!")
  }
}

// Run with: scala-cli HelloWorld.scala
```

### Modern Scala 3 Syntax

```scala
// Using @main annotation (Scala 3)
@main def hello(): Unit =
  println("Hello, Scala 3!")

// Even simpler
@main def greet(name: String): Unit =
  println(s"Hello, $name!")
```

## Basic Syntax Elements

```scala
// Variables
val immutable = 42        // Immutable (preferred)
var mutable = "Hello"     // Mutable

// Type inference
val number = 42           // Int inferred
val text = "Scala"        // String inferred
val pi = 3.14159         // Double inferred

// Explicit types
val count: Int = 10
val name: String = "Alice"
val scores: List[Int] = List(90, 85, 92)

// Functions
def add(x: Int, y: Int): Int = x + y
def greet(name: String): Unit = println(s"Hello, $name!")

// Single expression functions
def square(x: Int) = x * x

// Classes
class Person(val name: String, var age: Int) {
  def birthday(): Unit = age += 1
}

// Objects (singletons)
object Calculator {
  def add(x: Int, y: Int): Int = x + y
}

// Case classes (data classes)
case class Point(x: Int, y: Int)

// Pattern matching
val result = number match {
  case 0 => "zero"
  case 1 => "one"
  case _ => "many"
}
```

## Scala vs Other Languages

| Feature | Scala | Java | Python | Kotlin |
|---------|-------|------|--------|--------|
| Type System | Static, Strong | Static, Strong | Dynamic | Static, Strong |
| Paradigm | OOP + FP | OOP | Multi | OOP + FP |
| Null Safety | Option type | No | No | Built-in |
| Pattern Matching | Yes | Limited (14+) | Limited | Yes |
| Type Inference | Yes | Limited | N/A | Yes |
| Immutability | Default | No | No | val/var |

## Common Use Cases

1. **Big Data**: Apache Spark, Kafka
2. **Web Services**: Play Framework, Akka HTTP
3. **Reactive Systems**: Akka actors
4. **Data Science**: Spark MLlib
5. **Microservices**: Lightweight, functional services
6. **Domain Modeling**: Type-safe business logic

## REPL (Read-Eval-Print Loop)

Scala comes with an interactive shell:

```scala
$ scala-cli repl
Welcome to Scala 3.3.0
scala> val x = 42
val x: Int = 42

scala> def double(n: Int) = n * 2
def double(n: Int): Int

scala> double(x)
val res0: Int = 84

scala> List(1, 2, 3).map(_ * 2)
val res1: List[Int] = List(2, 4, 6)

scala> :quit
```

## Comments

```scala
// Single line comment

/* 
 * Multi-line
 * comment
 */

/** 
 * Documentation comment
 * @param name The person's name
 * @return A greeting string
 */
def greet(name: String): String = s"Hello, $name!"
```

## Next Steps

Now that you have Scala installed and understand the basics:
- Explore Scala's type system and variables
- Learn about functional programming concepts
- Understand pattern matching
- Master Scala collections
- Dive into concurrent programming with Futures and Akka
