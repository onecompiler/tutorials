# Classes and Objects in Scala

Scala is a pure object-oriented language where every value is an object. It provides powerful features for object-oriented programming while maintaining functional programming capabilities.

## Classes

### Basic Class Definition

```scala
class Person(firstName: String, lastName: String) {
  // Class body
  def fullName: String = s"$firstName $lastName"
  
  def greet(): Unit = {
    println(s"Hello, I'm $fullName")
  }
}

// Creating instances
val person = new Person("John", "Doe")
person.greet()  // Hello, I'm John Doe
```

### Class Parameters and Fields

```scala
// Parameters without val/var are private
class Student(name: String, grade: Int)

// Parameters with val become public immutable fields
class Employee(val name: String, val id: Int)

// Parameters with var become public mutable fields
class Counter(var count: Int) {
  def increment(): Unit = count += 1
}

// Mixed visibility
class BankAccount(val accountNumber: String, private var balance: Double) {
  def deposit(amount: Double): Unit = {
    if (amount > 0) balance += amount
  }
  
  def getBalance: Double = balance
}
```

### Constructors

```scala
class Rectangle(val width: Double, val height: Double) {
  // Primary constructor body
  println(s"Creating rectangle ${width}x${height}")
  
  // Auxiliary constructors must call primary constructor
  def this(size: Double) = {
    this(size, size)  // Square
  }
  
  def this() = {
    this(1.0)  // Unit square
  }
  
  def area: Double = width * height
}

val rect1 = new Rectangle(3, 4)
val square = new Rectangle(5)
val unit = new Rectangle()
```

### Methods and Properties

```scala
class Circle(radius: Double) {
  // Computed property (no parentheses)
  def area: Double = math.Pi * radius * radius
  
  // Method with side effects (use parentheses)
  def printInfo(): Unit = {
    println(s"Circle with radius $radius")
  }
  
  // Method with parameters
  def scale(factor: Double): Circle = {
    new Circle(radius * factor)
  }
  
  // Operator as method
  def +(other: Circle): Circle = {
    new Circle(radius + other.radius)
  }
}

val c1 = new Circle(5)
val c2 = new Circle(3)
val c3 = c1 + c2  // Using + operator
```

## Objects (Singletons)

### Singleton Objects

```scala
object MathConstants {
  val Pi = 3.14159
  val E = 2.71828
  
  def goldenRatio: Double = (1 + math.sqrt(5)) / 2
}

// Usage
println(MathConstants.Pi)
println(MathConstants.goldenRatio)
```

### Companion Objects

```scala
class User private(val id: Int, val name: String) {
  override def toString: String = s"User($id, $name)"
}

object User {
  // Factory methods
  def apply(name: String): User = new User(generateId(), name)
  
  def fromDatabase(id: Int): Option[User] = {
    // Simulate database lookup
    if (id > 0) Some(new User(id, s"User$id"))
    else None
  }
  
  private var nextId = 1
  private def generateId(): Int = {
    val id = nextId
    nextId += 1
    id
  }
}

// Usage
val user1 = User("Alice")  // Calls apply method
val user2 = User.fromDatabase(42)
```

## Case Classes

Case classes are immutable data containers with built-in features:

```scala
case class Point(x: Double, y: Double) {
  def distanceToOrigin: Double = math.sqrt(x * x + y * y)
  
  def move(dx: Double, dy: Double): Point = {
    Point(x + dx, y + dy)
  }
}

// Automatic features:
val p1 = Point(3, 4)  // No 'new' keyword needed
val p2 = p1.copy(y = 5)  // Copy with modifications
println(p1)  // Point(3.0,4.0) - toString implemented
val Point(x, y) = p1  // Pattern matching extraction

// Equality based on values
val p3 = Point(3, 4)
println(p1 == p3)  // true (structural equality)
```

### Case Classes vs Regular Classes

```scala
// Case class
case class Book(title: String, author: String, year: Int)

// Equivalent regular class (much more verbose!)
class RegularBook(val title: String, val author: String, val year: Int) {
  override def equals(obj: Any): Boolean = obj match {
    case that: RegularBook =>
      this.title == that.title && 
      this.author == that.author && 
      this.year == that.year
    case _ => false
  }
  
  override def hashCode(): Int = {
    val state = Seq(title, author, year)
    state.map(_.hashCode()).foldLeft(0)((a, b) => 31 * a + b)
  }
  
  override def toString: String = s"RegularBook($title,$author,$year)"
  
  def copy(title: String = this.title, 
           author: String = this.author, 
           year: Int = this.year): RegularBook = {
    new RegularBook(title, author, year)
  }
}
```

## Inheritance

### Class Inheritance

```scala
// Base class
open class Animal(val name: String) {
  def speak(): String = s"$name makes a sound"
  def move(): String = s"$name moves"
}

// Derived class
class Dog(name: String, val breed: String) extends Animal(name) {
  override def speak(): String = s"$name barks"
  
  def wagTail(): String = s"$name wags tail"
}

class Cat(name: String) extends Animal(name) {
  override def speak(): String = s"$name meows"
  override def move(): String = s"$name prowls"
}

val dog = new Dog("Rex", "Labrador")
println(dog.speak())  // Rex barks
println(dog.move())   // Rex moves (inherited)
```

### Abstract Classes

```scala
abstract class Shape {
  def area: Double  // Abstract method
  def perimeter: Double  // Abstract method
  
  // Concrete method
  def description: String = s"Shape with area $area and perimeter $perimeter"
}

class Rectangle(width: Double, height: Double) extends Shape {
  def area: Double = width * height
  def perimeter: Double = 2 * (width + height)
}

class Circle(radius: Double) extends Shape {
  def area: Double = math.Pi * radius * radius
  def perimeter: Double = 2 * math.Pi * radius
}
```

## Traits

Traits are like interfaces with implementation:

```scala
trait Greetable {
  def name: String  // Abstract
  
  def greet(): String = s"Hello, I'm $name"  // Concrete
}

trait Timestamped {
  val createdAt: Long = System.currentTimeMillis()
  
  def age: Long = System.currentTimeMillis() - createdAt
}

// Multiple trait inheritance
class Person(val name: String) extends Greetable with Timestamped {
  override def greet(): String = super.greet() + s" (created ${age}ms ago)"
}

// Traits with parameters (Scala 3)
trait Logger(prefix: String) {
  def log(message: String): Unit = println(s"$prefix: $message")
}
```

### Mixing Traits

```scala
trait Flying {
  def fly(): String = "Flying through the air"
}

trait Swimming {
  def swim(): String = "Swimming in water"
}

trait Running {
  def run(): String = "Running on ground"
}

class Duck extends Animal("Duck") with Flying with Swimming {
  override def speak(): String = "Quack!"
}

class Penguin extends Animal("Penguin") with Swimming with Running

val duck = new Duck
println(duck.fly())   // Flying through the air
println(duck.swim())  // Swimming in water
```

## Access Modifiers

```scala
class AccessExample {
  // Public (default)
  val publicField = "Everyone can see this"
  def publicMethod(): Unit = println("Public method")
  
  // Private - only this instance
  private val privateField = "Only this class"
  private def privateMethod(): Unit = println("Private method")
  
  // Protected - this class and subclasses
  protected val protectedField = "Subclasses can see"
  protected def protectedMethod(): Unit = println("Protected method")
  
  // Private to package
  private[mypackage] val packagePrivate = "Package visible"
  
  // Private to enclosing class
  class Inner {
    private[AccessExample] val outerPrivate = "Outer class can see"
  }
}
```

## Type Members

```scala
class Graph {
  // Type members
  type Node = Int
  type Edge = (Node, Node)
  
  private var nodes: Set[Node] = Set()
  private var edges: Set[Edge] = Set()
  
  def addNode(n: Node): Unit = nodes += n
  def addEdge(e: Edge): Unit = edges += e
}

// Path-dependent types
val g1 = new Graph
val g2 = new Graph
// g1.Node and g2.Node are considered the same type
```

## Inner Classes

```scala
class Outer(name: String) {
  outer =>  // Self reference
  
  class Inner(innerName: String) {
    def fullName: String = s"${outer.name}.$innerName"
  }
  
  def createInner(innerName: String): Inner = new Inner(innerName)
}

val outer1 = new Outer("First")
val outer2 = new Outer("Second")

val inner1 = outer1.createInner("A")
val inner2 = outer2.createInner("B")

// Path-dependent types: inner1 and inner2 have different types!
// outer1.Inner != outer2.Inner
```

## Enumerations

### Scala 2 Style

```scala
object Color extends Enumeration {
  val Red, Green, Blue = Value
  val Yellow = Value(10, "YELLOW")  // Custom id and name
}

// Usage
val color = Color.Red
println(color.id)  // 0
println(Color.values)  // Color.ValueSet(Red, Green, Blue, Yellow)
```

### Scala 3 Enums

```scala
enum Color {
  case Red, Green, Blue
  case Custom(hex: String)
}

// With methods
enum Planet(mass: Double, radius: Double) {
  case Mercury extends Planet(3.303e+23, 2.4397e6)
  case Earth extends Planet(5.976e+24, 6.37814e6)
  case Jupiter extends Planet(1.9e+27, 7.1492e7)
  
  def surfaceGravity: Double = 6.67300E-11 * mass / (radius * radius)
}
```

## Variance

```scala
// Covariance (+T) - if A <: B then Container[A] <: Container[B]
class Box[+T](val content: T)

// Contravariance (-T) - if A <: B then Container[B] <: Container[A]
trait Printer[-T] {
  def print(value: T): Unit
}

// Invariance (T) - no subtype relationship
class MutableBox[T](var content: T)

// Example
class Animal
class Dog extends Animal

val dogBox: Box[Dog] = new Box(new Dog)
val animalBox: Box[Animal] = dogBox  // OK because Box is covariant
```

## Best Practices

1. **Prefer immutability**: Use `val` and case classes
2. **Use meaningful names**: Classes should be nouns, methods should be verbs
3. **Keep classes focused**: Single Responsibility Principle
4. **Favor composition**: Use traits for reusable behavior
5. **Make classes final**: Unless designed for inheritance
6. **Use factory methods**: Companion object's apply method
7. **Override toString**: For better debugging

## Common Patterns

### Builder Pattern

```scala
case class Pizza(size: String = "medium", 
                 cheese: Boolean = true, 
                 toppings: List[String] = Nil) {
  
  def withSize(s: String): Pizza = copy(size = s)
  def withCheese(c: Boolean): Pizza = copy(cheese = c)
  def withTopping(t: String): Pizza = copy(toppings = t :: toppings)
}

val myPizza = Pizza()
  .withSize("large")
  .withTopping("mushrooms")
  .withTopping("pepperoni")
```

### Factory Pattern

```scala
trait Animal {
  def speak(): String
}

object Animal {
  private class Dog extends Animal {
    def speak(): String = "Woof"
  }
  
  private class Cat extends Animal {
    def speak(): String = "Meow"
  }
  
  def apply(animalType: String): Option[Animal] = animalType.toLowerCase match {
    case "dog" => Some(new Dog)
    case "cat" => Some(new Cat)
    case _ => None
  }
}

Animal("dog").foreach(_.speak())  // Woof
```