# Data Types in Scala

Scala has a rich type system that helps catch errors at compile time. Everything in Scala is an object, including numbers and functions. Scala's type hierarchy starts with `Any` at the top.

## Type Hierarchy

```
Any
├── AnyVal (Value Types)
│   ├── Byte
│   ├── Short
│   ├── Int
│   ├── Long
│   ├── Float
│   ├── Double
│   ├── Char
│   ├── Boolean
│   └── Unit
└── AnyRef (Reference Types)
    ├── String
    ├── Classes
    ├── Traits
    └── Null

Nothing (subtype of everything)
```

## Numeric Types

| Type | Description | Range | Size | Example |
|------|-------------|-------|------|---------|
| Byte | 8-bit signed integer | -128 to 127 | 1 byte | `val b: Byte = 100` |
| Short | 16-bit signed integer | -32,768 to 32,767 | 2 bytes | `val s: Short = 30000` |
| Int | 32-bit signed integer | -2³¹ to 2³¹-1 | 4 bytes | `val i: Int = 42` |
| Long | 64-bit signed integer | -2⁶³ to 2⁶³-1 | 8 bytes | `val l: Long = 42L` |
| Float | 32-bit IEEE 754 single-precision | ~±3.4×10³⁸ | 4 bytes | `val f: Float = 3.14f` |
| Double | 64-bit IEEE 754 double-precision | ~±1.7×10³⁰⁸ | 8 bytes | `val d: Double = 3.14159` |

### Numeric Literals and Operations

```scala
// Integer literals
val decimal = 42          // Decimal
val hex = 0x2A           // Hexadecimal (42)
val binary = 0b101010    // Binary (42) - Scala 2.13+

// Long literals
val longNumber = 42L
val bigLong = 100_000_000L  // Underscores for readability (Scala 2.13+)

// Floating-point literals
val float = 3.14f
val double = 3.14159
val scientific = 1.5e-3  // 0.0015
val bigDouble = 1_234.567_89  // Underscores allowed

// Numeric operations
val sum = 10 + 5         // 15
val product = 10 * 5     // 50
val quotient = 10 / 3    // 3 (integer division)
val remainder = 10 % 3   // 1
val power = math.pow(2, 3)  // 8.0
```

## Boolean Type

```scala
val isActive: Boolean = true
val isDeleted: Boolean = false

// Boolean operations
val and = true && false    // false
val or = true || false     // true
val not = !true           // false

// Comparison operations return Boolean
val isGreater = 10 > 5    // true
val isEqual = 42 == 42    // true
val notEqual = 10 != 5    // true
```

## Character Type

```scala
val letter: Char = 'A'
val digit: Char = '9'
val unicode: Char = '\u0041'  // 'A'
val tab: Char = '\t'
val newline: Char = '\n'

// Character operations
val nextChar = (letter + 1).toChar  // 'B'
val isDigit = digit.isDigit         // true
val isLetter = letter.isLetter      // true
val toLower = letter.toLower        // 'a'
```

### Escape Sequences

| Escape Sequence | Description |
|----------------|-------------|
| `\n` | Newline |
| `\t` | Tab |
| `\r` | Carriage return |
| `\b` | Backspace |
| `\f` | Form feed |
| `\\` | Backslash |
| `\'` | Single quote |
| `\"` | Double quote |
| `\u0000` | Unicode character |

## String Type

Strings in Scala are immutable sequences of characters:

```scala
// String literals
val name = "Scala"
val empty = ""

// String interpolation
val version = 3
val message = s"Welcome to Scala $version"  // "Welcome to Scala 3"
val calculation = s"2 + 2 = ${2 + 2}"      // "2 + 2 = 4"

// Raw strings (no escape processing)
val path = raw"C:\Users\name\Documents"    // Backslashes not escaped

// Formatted strings
val pi = 3.14159
val formatted = f"Pi is approximately $pi%.2f"  // "Pi is approximately 3.14"

// Multi-line strings
val multiline = """
  |This is a
  |multi-line string
  |with proper formatting
  """.stripMargin

// String operations
val upper = name.toUpperCase      // "SCALA"
val length = name.length          // 5
val concat = name + " rocks!"     // "Scala rocks!"
val contains = name.contains("ca") // true
val substring = name.substring(1, 4) // "cal"
```

## Unit Type

Unit is Scala's equivalent of void, representing no value:

```scala
def printMessage(msg: String): Unit = {
  println(msg)
}

val unit: Unit = ()  // The only value of type Unit
```

## Null, Nil, None, Nothing

### Null
```scala
val str: String = null  // Avoid using null in Scala!
// Use Option instead
```

### Nil
```scala
val emptyList: List[Int] = Nil
val list = 1 :: 2 :: 3 :: Nil  // List(1, 2, 3)
```

### None and Some (Option type)
```scala
val maybeNumber: Option[Int] = Some(42)
val noNumber: Option[Int] = None

// Safe handling of potentially missing values
def findUser(id: Int): Option[String] = {
  if (id == 1) Some("Alice") else None
}
```

### Nothing
```scala
// Nothing is the bottom type - subtype of everything
def error(msg: String): Nothing = {
  throw new Exception(msg)
}
```

## Type Inference

Scala can often infer types, making code more concise:

```scala
// Explicit type annotations
val number: Int = 42
val text: String = "Hello"
val list: List[Int] = List(1, 2, 3)

// Type inference
val number = 42          // Int inferred
val text = "Hello"       // String inferred
val list = List(1, 2, 3) // List[Int] inferred

// Sometimes explicit types are needed
val empty = List()       // List[Nothing] - not useful
val empty: List[Int] = List()  // List[Int] - better
```

## Type Casting

```scala
// Numeric conversions
val int: Int = 42
val long: Long = int.toLong
val double: Double = int.toDouble
val string: String = int.toString

// Downcasting (be careful!)
val bigNumber: Long = 1000000L
val smallNumber: Int = bigNumber.toInt  // Possible data loss

// Safe casting with pattern matching
val any: Any = "Hello"
any match {
  case s: String => println(s"It's a string: $s")
  case i: Int => println(s"It's an int: $i")
  case _ => println("Unknown type")
}
```

## Rich Wrapper Classes

Scala provides rich wrapper classes that add methods to basic types:

```scala
// RichInt
val n = 5
n.to(10)        // Range(5, 6, 7, 8, 9, 10)
n.until(10)     // Range(5, 6, 7, 8, 9)
n.max(10)       // 10
n.min(10)       // 5

// RichDouble
val d = 3.14
d.ceil          // 4.0
d.floor         // 3.0
d.round         // 3

// RichChar
val c = 'a'
c.to('e')       // NumericRange(a, b, c, d, e)
c.isLower       // true
c.isDigit       // false
```

## Value Classes

For performance optimization, you can create value classes:

```scala
// Value class - no runtime overhead
class Age(val value: Int) extends AnyVal {
  def isAdult: Boolean = value >= 18
}

val age = new Age(25)
println(age.isAdult)  // true
```

## Type Aliases

Create readable names for complex types:

```scala
type UserId = Int
type UserName = String
type UserMap = Map[UserId, UserName]

def getUser(id: UserId): Option[UserName] = {
  val users: UserMap = Map(1 -> "Alice", 2 -> "Bob")
  users.get(id)
}
```

## Best Practices

1. **Prefer Immutability**: Use `val` over `var`
2. **Avoid Null**: Use `Option` for potentially missing values
3. **Use Type Inference**: But add types for public APIs
4. **Be Explicit When Needed**: Complex types benefit from annotations
5. **Use String Interpolation**: More readable than concatenation
6. **Leverage Rich Wrappers**: They provide many useful methods

