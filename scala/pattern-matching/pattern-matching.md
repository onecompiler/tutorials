# Pattern Matching in Scala

Pattern matching is one of Scala's most powerful features, allowing you to match values against patterns and extract data in a concise, readable way. It's like a switch statement on steroids.

## Basic Pattern Matching

### Match Expression

```scala
def describe(x: Any): String = x match {
  case 5 => "five"
  case true => "truth"
  case "hello" => "hi!"
  case Nil => "the empty list"
  case _ => "something else"  // Default case
}

println(describe(5))       // "five"
println(describe(true))    // "truth"
println(describe("hi"))    // "something else"
```

### Pattern Guards

Add conditions to patterns:

```scala
def checkNumber(x: Int): String = x match {
  case n if n > 0 => "positive"
  case n if n < 0 => "negative"
  case _ => "zero"
}

def isEven(n: Int): String = n match {
  case x if x % 2 == 0 => s"$x is even"
  case x => s"$x is odd"
}
```

## Type Patterns

Match based on type:

```scala
def processValue(value: Any): String = value match {
  case s: String => s"String of length ${s.length}"
  case i: Int => s"Int squared is ${i * i}"
  case d: Double => s"Double rounded is ${d.round}"
  case list: List[_] => s"List with ${list.size} elements"
  case _ => "Unknown type"
}

println(processValue("hello"))     // "String of length 5"
println(processValue(42))          // "Int squared is 1764"
println(processValue(List(1,2,3))) // "List with 3 elements"
```

## Case Classes and Pattern Matching

Case classes are perfect for pattern matching:

```scala
// Define case classes
sealed trait Animal
case class Dog(name: String, breed: String) extends Animal
case class Cat(name: String, livesLeft: Int) extends Animal
case class Bird(species: String) extends Animal

// Pattern match on case classes
def describeAnimal(animal: Animal): String = animal match {
  case Dog(name, breed) => s"$name is a $breed"
  case Cat(name, lives) => s"$name has $lives lives left"
  case Bird(species) => s"A $species bird"
}

val myDog = Dog("Rex", "German Shepherd")
println(describeAnimal(myDog))  // "Rex is a German Shepherd"
```

### Extracting Values

```scala
case class Person(name: String, age: Int)

def greetPerson(person: Person): String = person match {
  case Person("Alice", _) => "Hi Alice!"
  case Person(name, age) if age < 18 => s"Hey $name!"
  case Person(name, age) => s"Hello Mr/Ms $name"
}

// Extracting with variable binding
def getInfo(person: Person): String = person match {
  case p @ Person(_, age) if age >= 65 => s"Senior: $p"
  case Person(name, _) => s"Person named $name"
}
```

## Collection Patterns

### List Patterns

```scala
def describeList(list: List[Int]): String = list match {
  case Nil => "Empty list"
  case head :: Nil => s"Single element: $head"
  case head :: tail => s"Head: $head, Tail: $tail"
}

// More complex patterns
def sumFirstTwo(list: List[Int]): Int = list match {
  case first :: second :: _ => first + second
  case first :: Nil => first
  case Nil => 0
}

// Pattern matching with specific values
def startsWithOne(list: List[Int]): Boolean = list match {
  case 1 :: _ => true
  case _ => false
}
```

### Array Patterns

```scala
def describeArray(arr: Array[Int]): String = arr match {
  case Array() => "Empty array"
  case Array(x) => s"Single element: $x"
  case Array(x, y) => s"Two elements: $x and $y"
  case Array(x, y, _*) => s"At least two elements starting with $x and $y"
}
```

### Tuple Patterns

```scala
def processPair(pair: (String, Int)): String = pair match {
  case ("error", code) => s"Error with code $code"
  case (name, 0) => s"$name has zero value"
  case (name, value) => s"$name: $value"
}

// Multiple tuple elements
def processTriple(triple: (Int, Int, Int)): String = triple match {
  case (x, y, z) if x == y && y == z => "All equal"
  case (x, y, _) if x == y => "First two equal"
  case (0, 0, 0) => "Origin"
  case _ => "Other"
}
```

## Option Pattern Matching

Working with Option types:

```scala
def processOption(opt: Option[Int]): String = opt match {
  case Some(n) if n > 0 => s"Positive: $n"
  case Some(0) => "Zero"
  case Some(n) => s"Negative: $n"
  case None => "No value"
}

// Using map/flatMap vs pattern matching
val result = Some(42)
val doubled = result match {
  case Some(n) => Some(n * 2)
  case None => None
}
// Equivalent to: result.map(_ * 2)
```

## Pattern Matching in Variable Definitions

```scala
// Tuple decomposition
val (name, age) = ("Alice", 30)

// Case class decomposition
case class Point(x: Int, y: Int)
val Point(xCoord, yCoord) = Point(10, 20)

// List decomposition
val head :: tail = List(1, 2, 3, 4)
// head = 1, tail = List(2, 3, 4)

// For comprehension with patterns
val pairs = List((1, "one"), (2, "two"), (3, "three"))
for ((num, word) <- pairs) {
  println(s"$num is written as $word")
}
```

## Sealed Traits and Exhaustiveness

```scala
sealed trait Result[+T]
case class Success[T](value: T) extends Result[T]
case class Failure(error: String) extends Result[Nothing]

def processResult[T](result: Result[T]): String = result match {
  case Success(value) => s"Success: $value"
  case Failure(error) => s"Failed: $error"
  // Compiler warns if we miss a case!
}
```

## Advanced Patterns

### Nested Patterns

```scala
case class Address(street: String, city: String)
case class Employee(name: String, address: Address)

def getCity(employee: Employee): String = employee match {
  case Employee(_, Address(_, city)) => city
}

// Deep matching
def isInNYC(employee: Employee): Boolean = employee match {
  case Employee(_, Address(_, "New York")) => true
  case _ => false
}
```

### Pattern Sequences

```scala
// Variable-length argument patterns
def sum(numbers: List[Int]): Int = numbers match {
  case Nil => 0
  case head :: tail => head + sum(tail)
}

// Matching specific patterns in sequences
def hasConsecutive(list: List[Int]): Boolean = list match {
  case _ :: x :: y :: _ if y == x + 1 => true
  case _ :: tail => hasConsecutive(tail)
  case _ => false
}
```

### Regular Expression Patterns

```scala
val emailPattern = """(\w+)@(\w+\.\w+)""".r

def extractEmail(text: String): Option[(String, String)] = text match {
  case emailPattern(user, domain) => Some((user, domain))
  case _ => None
}

println(extractEmail("alice@example.com"))  // Some((alice, example.com))
```

## Pattern Matching in Partial Functions

```scala
val divide: PartialFunction[Int, Int] = {
  case d if d != 0 => 42 / d
}

if (divide.isDefinedAt(6)) {
  println(divide(6))  // 7
}

// Combining partial functions
val positiveSquare: PartialFunction[Int, Int] = {
  case x if x > 0 => x * x
}

val negativeDouble: PartialFunction[Int, Int] = {
  case x if x < 0 => x * 2
}

val combined = positiveSquare orElse negativeDouble
```

## Pattern Matching Best Practices

### 1. Order Matters

```scala
def matchExample(x: Any): String = x match {
  case _: String => "Any string"    // This would catch all strings
  case "specific" => "Never reached" // This is unreachable!
}
```

### 2. Use Sealed Traits

```scala
sealed trait Color
case object Red extends Color
case object Green extends Color
case object Blue extends Color

// Compiler ensures exhaustiveness
def describe(color: Color): String = color match {
  case Red => "Red"
  case Green => "Green"
  case Blue => "Blue"
  // No default case needed - compiler knows all cases are covered
}
```

### 3. Avoid Type Erasure Issues

```scala
def problematic(x: Any): String = x match {
  case _: List[Int] => "List of Ints"    // Warning: type erasure!
  case _: List[String] => "List of Strings" // Won't work as expected
}

// Better approach
def better(x: Any): String = x match {
  case list: List[_] => s"List of ${list.size} elements"
  case _ => "Not a list"
}
```

## Real-World Examples

### JSON-like Data Processing

```scala
sealed trait JsonValue
case class JsonObject(fields: Map[String, JsonValue]) extends JsonValue
case class JsonArray(items: List[JsonValue]) extends JsonValue
case class JsonString(value: String) extends JsonValue
case class JsonNumber(value: Double) extends JsonValue
case class JsonBoolean(value: Boolean) extends JsonValue
case object JsonNull extends JsonValue

def stringify(json: JsonValue): String = json match {
  case JsonObject(fields) => 
    fields.map { case (k, v) => s""""$k": ${stringify(v)}""" }
          .mkString("{", ", ", "}")
  case JsonArray(items) => 
    items.map(stringify).mkString("[", ", ", "]")
  case JsonString(s) => s""""$s""""
  case JsonNumber(n) => n.toString
  case JsonBoolean(b) => b.toString
  case JsonNull => "null"
}
```

### State Machine

```scala
sealed trait State
case object Idle extends State
case object Running extends State
case object Stopped extends State

sealed trait Event
case object Start extends Event
case object Stop extends Event
case object Reset extends Event

def transition(state: State, event: Event): State = (state, event) match {
  case (Idle, Start) => Running
  case (Running, Stop) => Stopped
  case (Stopped, Reset) => Idle
  case (s, _) => s  // No transition
}
```

## Common Pitfalls

1. **Forgetting the default case**: Can cause MatchError at runtime
2. **Pattern order**: More specific patterns should come first
3. **Variable shadowing**: Be careful with variable names in patterns
4. **Type erasure**: Generic types are erased at runtime

## Performance Considerations

Pattern matching compiles to efficient bytecode, typically using:
- `tableswitch` or `lookupswitch` for simple matches
- `instanceof` checks for type patterns
- Method calls for extractors

For performance-critical code, simple if-else chains might be slightly faster, but pattern matching is usually preferred for readability.