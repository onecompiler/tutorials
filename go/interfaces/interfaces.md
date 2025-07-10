# Go Interfaces

## Introduction

Interfaces are one of the most powerful features in Go. They provide a way to specify the behavior of an object: if something can do *this*, then it can be used *here*. Interfaces are types that define a set of method signatures without implementing them. They enable polymorphism and help write flexible, testable, and maintainable code.

## What are Interfaces in Go?

An interface in Go is a type that specifies a set of method signatures. A type implements an interface by implementing all the methods in the interface. Unlike many other languages, Go interfaces are satisfied implicitly — there's no explicit declaration of intent.

### Key Characteristics:
- Interfaces define behavior, not data
- A type implements an interface by implementing its methods
- Interface implementation is implicit (no "implements" keyword)
- Interfaces enable duck typing: "If it walks like a duck and quacks like a duck, it's a duck"

## Interface Declaration and Implementation

### Basic Interface Declaration

```go
package main

import "fmt"

// Shape is an interface with two methods
type Shape interface {
    Area() float64
    Perimeter() float64
}

// Rectangle type
type Rectangle struct {
    Width  float64
    Height float64
}

// Rectangle implements Shape interface
func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}

func (r Rectangle) Perimeter() float64 {
    return 2 * (r.Width + r.Height)
}

// Circle type
type Circle struct {
    Radius float64
}

// Circle implements Shape interface
func (c Circle) Area() float64 {
    return 3.14159 * c.Radius * c.Radius
}

func (c Circle) Perimeter() float64 {
    return 2 * 3.14159 * c.Radius
}

// Function that accepts any Shape
func PrintShapeInfo(s Shape) {
    fmt.Printf("Area: %.2f, Perimeter: %.2f\n", s.Area(), s.Perimeter())
}

func main() {
    rect := Rectangle{Width: 10, Height: 5}
    circle := Circle{Radius: 7}
    
    PrintShapeInfo(rect)   // Area: 50.00, Perimeter: 30.00
    PrintShapeInfo(circle) // Area: 153.94, Perimeter: 43.98
}
```

### Multiple Interface Implementation

A type can implement multiple interfaces:

```go
package main

import "fmt"

type Writer interface {
    Write([]byte) (int, error)
}

type Closer interface {
    Close() error
}

type File struct {
    name string
}

func (f *File) Write(data []byte) (int, error) {
    fmt.Printf("Writing %d bytes to %s\n", len(data), f.name)
    return len(data), nil
}

func (f *File) Close() error {
    fmt.Printf("Closing file %s\n", f.name)
    return nil
}

func main() {
    f := &File{name: "test.txt"}
    
    // File implements both Writer and Closer
    var w Writer = f
    var c Closer = f
    
    w.Write([]byte("Hello"))
    c.Close()
}
```

## Empty Interface (interface{})

The empty interface `interface{}` specifies zero methods and is satisfied by any type. It's Go's way of representing "any type".

```go
package main

import "fmt"

func PrintAnything(v interface{}) {
    fmt.Printf("Type: %T, Value: %v\n", v, v)
}

func main() {
    PrintAnything(42)           // Type: int, Value: 42
    PrintAnything("hello")      // Type: string, Value: hello
    PrintAnything(3.14)         // Type: float64, Value: 3.14
    PrintAnything([]int{1, 2})  // Type: []int, Value: [1 2]
    
    // Storing different types in a slice
    var things []interface{}
    things = append(things, 42)
    things = append(things, "hello")
    things = append(things, true)
    
    for _, thing := range things {
        fmt.Println(thing)
    }
}
```

### Note on Go 1.18+
With Go 1.18 and later, `any` is an alias for `interface{}`:

```go
func PrintAnything(v any) { // any is equivalent to interface{}
    fmt.Printf("Type: %T, Value: %v\n", v, v)
}
```

## Type Assertions and Type Switches

### Type Assertions

Type assertions provide access to an interface value's underlying concrete value:

```go
package main

import "fmt"

func main() {
    var i interface{} = "hello"
    
    // Type assertion
    s := i.(string)
    fmt.Println(s) // hello
    
    // Safe type assertion with comma-ok idiom
    s, ok := i.(string)
    if ok {
        fmt.Println("String value:", s)
    }
    
    // This would panic without comma-ok
    // f := i.(float64) // panic: interface conversion
    
    // Safe way
    f, ok := i.(float64)
    if !ok {
        fmt.Println("Value is not a float64")
    }
}
```

### Type Switches

Type switches allow you to handle multiple types:

```go
package main

import "fmt"

func describe(i interface{}) {
    switch v := i.(type) {
    case int:
        fmt.Printf("Integer: %d\n", v)
    case string:
        fmt.Printf("String: %s (length: %d)\n", v, len(v))
    case bool:
        fmt.Printf("Boolean: %t\n", v)
    case float64:
        fmt.Printf("Float: %f\n", v)
    case []int:
        fmt.Printf("Slice of ints: %v (length: %d)\n", v, len(v))
    case nil:
        fmt.Println("nil value")
    default:
        fmt.Printf("Unknown type: %T\n", v)
    }
}

func main() {
    describe(42)              // Integer: 42
    describe("hello")         // String: hello (length: 5)
    describe(3.14)           // Float: 3.140000
    describe(true)           // Boolean: true
    describe([]int{1, 2, 3}) // Slice of ints: [1 2 3] (length: 3)
    describe(struct{}{})     // Unknown type: struct {}
}
```

## Common Interfaces

### Stringer Interface

The `fmt.Stringer` interface is used to define custom string representations:

```go
package main

import "fmt"

type Person struct {
    Name string
    Age  int
}

// Implement the Stringer interface
func (p Person) String() string {
    return fmt.Sprintf("%s (%d years old)", p.Name, p.Age)
}

func main() {
    p := Person{Name: "Alice", Age: 30}
    fmt.Println(p) // Alice (30 years old)
    
    // Works with any fmt function
    fmt.Printf("Person: %s\n", p) // Person: Alice (30 years old)
}
```

### Error Interface

The `error` interface is fundamental in Go error handling:

```go
package main

import (
    "fmt"
)

// Custom error type
type ValidationError struct {
    Field   string
    Message string
}

// Implement the error interface
func (e ValidationError) Error() string {
    return fmt.Sprintf("validation error on field '%s': %s", e.Field, e.Message)
}

func validateAge(age int) error {
    if age < 0 {
        return ValidationError{
            Field:   "age",
            Message: "age cannot be negative",
        }
    }
    if age > 150 {
        return ValidationError{
            Field:   "age",
            Message: "age seems unrealistic",
        }
    }
    return nil
}

func main() {
    if err := validateAge(-5); err != nil {
        fmt.Println(err) // validation error on field 'age': age cannot be negative
    }
    
    // Type assertion to get more details
    if err := validateAge(200); err != nil {
        if valErr, ok := err.(ValidationError); ok {
            fmt.Printf("Field: %s, Message: %s\n", valErr.Field, valErr.Message)
        }
    }
}
```

### io.Reader and io.Writer Interfaces

These are among the most important interfaces in Go's standard library:

```go
package main

import (
    "fmt"
    "io"
    "strings"
    "bytes"
)

// Custom type implementing io.Reader
type RotReader struct {
    reader io.Reader
}

func (r RotReader) Read(p []byte) (n int, err error) {
    n, err = r.reader.Read(p)
    // ROT13 transformation
    for i := 0; i < n; i++ {
        if p[i] >= 'A' && p[i] <= 'Z' {
            p[i] = 'A' + (p[i]-'A'+13)%26
        } else if p[i] >= 'a' && p[i] <= 'z' {
            p[i] = 'a' + (p[i]-'a'+13)%26
        }
    }
    return
}

// Function that works with any io.Reader
func ReadAndPrint(r io.Reader) error {
    buf := make([]byte, 1024)
    n, err := r.Read(buf)
    if err != nil && err != io.EOF {
        return err
    }
    fmt.Printf("Read %d bytes: %s\n", n, buf[:n])
    return nil
}

func main() {
    // strings.Reader implements io.Reader
    sr := strings.NewReader("Hello, World!")
    ReadAndPrint(sr) // Read 13 bytes: Hello, World!
    
    // bytes.Buffer implements io.Reader and io.Writer
    var buf bytes.Buffer
    buf.WriteString("Buffer content")
    ReadAndPrint(&buf) // Read 14 bytes: Buffer content
    
    // Custom reader with ROT13
    rot := RotReader{reader: strings.NewReader("Hello, World!")}
    ReadAndPrint(rot) // Read 13 bytes: Uryyb, Jbeyq!
}
```

### Example: Copy Function Using io.Reader and io.Writer

```go
package main

import (
    "io"
    "os"
    "strings"
)

// Copy data from reader to writer
func Copy(dst io.Writer, src io.Reader) (int64, error) {
    return io.Copy(dst, src)
}

func main() {
    // Copy from string to stdout
    reader := strings.NewReader("Hello from reader!\n")
    Copy(os.Stdout, reader) // Prints: Hello from reader!
    
    // Works with any types implementing the interfaces
    var buf bytes.Buffer
    reader2 := strings.NewReader("Copying to buffer")
    Copy(&buf, reader2)
    fmt.Println(buf.String()) // Copying to buffer
}
```

## Interface Composition

Interfaces can be composed from other interfaces:

```go
package main

import (
    "fmt"
    "io"
)

// Individual interfaces
type Reader interface {
    Read([]byte) (int, error)
}

type Writer interface {
    Write([]byte) (int, error)
}

type Closer interface {
    Close() error
}

// Composed interfaces
type ReadWriter interface {
    Reader
    Writer
}

type ReadWriteCloser interface {
    Reader
    Writer
    Closer
}

// This is equivalent to io.ReadWriteCloser
type ReadWriteCloser2 interface {
    io.Reader
    io.Writer
    io.Closer
}

// Example implementation
type File struct {
    name string
}

func (f *File) Read(p []byte) (int, error) {
    fmt.Printf("Reading from %s\n", f.name)
    return 0, io.EOF
}

func (f *File) Write(p []byte) (int, error) {
    fmt.Printf("Writing %d bytes to %s\n", len(p), f.name)
    return len(p), nil
}

func (f *File) Close() error {
    fmt.Printf("Closing %s\n", f.name)
    return nil
}

func ProcessFile(rwc ReadWriteCloser) {
    defer rwc.Close()
    
    rwc.Write([]byte("some data"))
    
    buf := make([]byte, 1024)
    rwc.Read(buf)
}

func main() {
    f := &File{name: "example.txt"}
    ProcessFile(f)
    // Output:
    // Writing 9 bytes to example.txt
    // Reading from example.txt
    // Closing example.txt
}
```

### Named Interface Composition

```go
package main

import "fmt"

type Animal interface {
    Name() string
    Sound() string
}

type Mover interface {
    Move() string
}

// Composed interface with additional method
type Pet interface {
    Animal
    Mover
    Play() string
}

type Dog struct {
    name string
}

func (d Dog) Name() string  { return d.name }
func (d Dog) Sound() string { return "Woof!" }
func (d Dog) Move() string  { return "Running" }
func (d Dog) Play() string  { return "Fetching ball" }

func DescribePet(p Pet) {
    fmt.Printf("%s says %s\n", p.Name(), p.Sound())
    fmt.Printf("%s is %s\n", p.Name(), p.Move())
    fmt.Printf("%s is %s\n", p.Name(), p.Play())
}

func main() {
    dog := Dog{name: "Buddy"}
    DescribePet(dog)
    // Output:
    // Buddy says Woof!
    // Buddy is Running
    // Buddy is Fetching ball
}
```

## Best Practices

### 1. Keep Interfaces Small

Prefer many small interfaces over few large ones:

```go
// Good - Small, focused interfaces
type Reader interface {
    Read([]byte) (int, error)
}

type Writer interface {
    Write([]byte) (int, error)
}

// Less ideal - Large interface
type FileHandler interface {
    Read([]byte) (int, error)
    Write([]byte) (int, error)
    Seek(int64, int) (int64, error)
    Close() error
    Stat() (FileInfo, error)
    Chmod(FileMode) error
    // ... many more methods
}
```

### 2. Accept Interfaces, Return Concrete Types

```go
// Good - Accept interface
func SaveData(w io.Writer, data []byte) error {
    _, err := w.Write(data)
    return err
}

// Good - Return concrete type
func OpenFile(name string) (*os.File, error) {
    return os.Open(name)
}

// Less ideal - Returning interface when concrete type would be better
func OpenFile2(name string) (io.ReadWriteCloser, error) {
    return os.Open(name)
}
```

### 3. Design Interfaces at the Point of Use

Don't define interfaces before you need them:

```go
// Define interface where it's used, not where types are defined
package consumer

type DataStore interface {
    Get(key string) (string, error)
    Set(key string, value string) error
}

func ProcessData(ds DataStore, key string) error {
    value, err := ds.Get(key)
    if err != nil {
        return err
    }
    // Process value...
    return ds.Set(key, processedValue)
}
```

### 4. Use Interface{} Sparingly

Prefer specific types when possible:

```go
// Less ideal - loses type safety
func Add(a, b interface{}) interface{} {
    // Need type assertions...
}

// Better - type safe
func AddInts(a, b int) int {
    return a + b
}

// Or use generics (Go 1.18+)
func Add[T ~int | ~float64](a, b T) T {
    return a + b
}
```

### 5. Pointer vs Value Receivers

Be consistent with receiver types:

```go
type Counter struct {
    count int
}

// If one method has pointer receiver, usually all should
func (c *Counter) Increment() {
    c.count++
}

func (c *Counter) Value() int {
    return c.count
}

// Interface is satisfied by *Counter, not Counter
type Incrementer interface {
    Increment()
    Value() int
}
```

### 6. Document Interface Contracts

```go
// Writer is the interface that wraps the basic Write method.
//
// Write writes len(p) bytes from p to the underlying data stream.
// It returns the number of bytes written from p (0 <= n <= len(p))
// and any error encountered that caused the write to stop early.
// Write must return a non-nil error if it returns n < len(p).
// Write must not modify the slice data, even temporarily.
type Writer interface {
    Write(p []byte) (n int, error)
}
```

### 7. Testing with Interfaces

Interfaces make testing easier:

```go
package main

import (
    "fmt"
    "errors"
)

type Database interface {
    Get(id string) (string, error)
    Set(id, value string) error
}

type Service struct {
    db Database
}

func (s *Service) GetUser(id string) (string, error) {
    return s.db.Get(id)
}

// Mock for testing
type MockDB struct {
    data map[string]string
}

func (m *MockDB) Get(id string) (string, error) {
    if val, ok := m.data[id]; ok {
        return val, nil
    }
    return "", errors.New("not found")
}

func (m *MockDB) Set(id, value string) error {
    m.data[id] = value
    return nil
}

func main() {
    // In tests, use mock
    mock := &MockDB{data: map[string]string{"1": "Alice"}}
    service := Service{db: mock}
    
    user, _ := service.GetUser("1")
    fmt.Println(user) // Alice
}
```

## Advanced Example: Plugin System

Here's a practical example showing how interfaces enable extensible design:

```go
package main

import (
    "fmt"
    "strings"
)

// Plugin interface
type TextProcessor interface {
    Process(text string) string
    Name() string
}

// Plugin implementations
type UppercaseProcessor struct{}

func (u UppercaseProcessor) Process(text string) string {
    return strings.ToUpper(text)
}

func (u UppercaseProcessor) Name() string {
    return "Uppercase"
}

type ReverseProcessor struct{}

func (r ReverseProcessor) Process(text string) string {
    runes := []rune(text)
    for i, j := 0, len(runes)-1; i < j; i, j = i+1, j-1 {
        runes[i], runes[j] = runes[j], runes[i]
    }
    return string(runes)
}

func (r ReverseProcessor) Name() string {
    return "Reverse"
}

type TrimProcessor struct{}

func (t TrimProcessor) Process(text string) string {
    return strings.TrimSpace(text)
}

func (t TrimProcessor) Name() string {
    return "Trim"
}

// Pipeline that uses multiple processors
type Pipeline struct {
    processors []TextProcessor
}

func (p *Pipeline) AddProcessor(proc TextProcessor) {
    p.processors = append(p.processors, proc)
}

func (p *Pipeline) Process(text string) string {
    result := text
    for _, proc := range p.processors {
        fmt.Printf("Applying %s processor\n", proc.Name())
        result = proc.Process(result)
    }
    return result
}

func main() {
    pipeline := &Pipeline{}
    
    // Add processors dynamically
    pipeline.AddProcessor(TrimProcessor{})
    pipeline.AddProcessor(UppercaseProcessor{})
    pipeline.AddProcessor(ReverseProcessor{})
    
    input := "  Hello, World!  "
    output := pipeline.Process(input)
    
    fmt.Printf("Input:  '%s'\n", input)
    fmt.Printf("Output: '%s'\n", output)
    // Output:
    // Applying Trim processor
    // Applying Uppercase processor
    // Applying Reverse processor
    // Input:  '  Hello, World!  '
    // Output: '!DLROW ,OLLEH'
}
```

## Summary

Interfaces are a cornerstone of Go's type system, enabling:

1. **Polymorphism**: Different types can be used interchangeably if they satisfy the same interface
2. **Decoupling**: Code can depend on behavior (interfaces) rather than concrete types
3. **Testability**: Easy to create mock implementations for testing
4. **Composition**: Build complex behaviors from simple interfaces
5. **Flexibility**: Add new types without changing existing code

Remember:
- Keep interfaces small and focused
- Define interfaces where they're used
- Accept interfaces, return concrete types
- Use empty interface sparingly
- Document interface contracts clearly
- Leverage interfaces for better testing

Interfaces are implicitly satisfied in Go, which means types automatically implement an interface if they have the required methods. This makes Go's interface system both powerful and easy to use, encouraging good design practices and clean, modular code.