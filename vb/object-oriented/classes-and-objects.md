# Classes and Objects in VB.NET

VB.NET is a fully object-oriented programming language that supports all OOP concepts including encapsulation, inheritance, and polymorphism. Understanding classes and objects is fundamental to mastering VB.NET.

## Classes

A class is a blueprint or template for creating objects. It defines the data (fields/properties) and behavior (methods) that objects of that type will have.

### Basic Class Definition

```vb
Public Class Person
    ' Fields
    Private _firstName As String
    Private _lastName As String
    
    ' Constructor
    Public Sub New(firstName As String, lastName As String)
        _firstName = firstName
        _lastName = lastName
    End Sub
    
    ' Property
    Public ReadOnly Property FullName As String
        Get
            Return $"{_firstName} {_lastName}"
        End Get
    End Property
    
    ' Method
    Public Sub Greet()
        Console.WriteLine($"Hello, I'm {FullName}")
    End Sub
End Class

' Creating and using objects
Dim person As New Person("John", "Doe")
person.Greet()  ' Output: Hello, I'm John Doe
```

### Class Members

```vb
Public Class Employee
    ' Fields (private by convention)
    Private _id As Integer
    Private _salary As Decimal
    
    ' Auto-implemented properties
    Public Property Name As String
    Public Property Department As String
    
    ' Property with custom getter/setter
    Public Property Salary As Decimal
        Get
            Return _salary
        End Get
        Set(value As Decimal)
            If value >= 0 Then
                _salary = value
            Else
                Throw New ArgumentException("Salary cannot be negative")
            End If
        End Set
    End Property
    
    ' Read-only property
    Public ReadOnly Property AnnualSalary As Decimal
        Get
            Return _salary * 12
        End Get
    End Property
    
    ' Methods
    Public Function GetInfo() As String
        Return $"Employee: {Name}, Dept: {Department}, Salary: {Salary:C}"
    End Function
    
    ' Shared (static) members
    Private Shared _nextId As Integer = 1000
    
    Public Shared Function GetNextId() As Integer
        _nextId += 1
        Return _nextId
    End Function
End Class
```

## Constructors

Constructors initialize new instances of a class:

```vb
Public Class Rectangle
    Public Property Width As Double
    Public Property Height As Double
    
    ' Default constructor
    Public Sub New()
        Width = 1
        Height = 1
    End Sub
    
    ' Parameterized constructor
    Public Sub New(width As Double, height As Double)
        Me.Width = width
        Me.Height = height
    End Sub
    
    ' Constructor overloading - square
    Public Sub New(size As Double)
        Me.New(size, size)  ' Call another constructor
    End Sub
    
    ' Shared constructor (runs once when type is first used)
    Shared Sub New()
        Console.WriteLine("Rectangle type initialized")
    End Sub
    
    Public Function Area() As Double
        Return Width * Height
    End Function
End Class

' Usage
Dim rect1 As New Rectangle()           ' 1x1
Dim rect2 As New Rectangle(3, 4)       ' 3x4
Dim square As New Rectangle(5)         ' 5x5
```

## Properties

Properties provide controlled access to class data:

```vb
Public Class BankAccount
    Private _balance As Decimal
    Private _accountNumber As String
    Private ReadOnly _createdDate As DateTime
    
    Public Sub New(accountNumber As String)
        _accountNumber = accountNumber
        _createdDate = DateTime.Now
        _balance = 0
    End Sub
    
    ' Read-only property
    Public ReadOnly Property AccountNumber As String
        Get
            Return _accountNumber
        End Get
    End Property
    
    ' Read-write property with validation
    Public Property Balance As Decimal
        Get
            Return _balance
        End Get
        Private Set(value As Decimal)  ' Private setter
            _balance = value
        End Set
    End Property
    
    ' Write-only property (rare)
    Public WriteOnly Property Pin As String
        Set(value As String)
            If value.Length = 4 AndAlso value.All(AddressOf Char.IsDigit) Then
                ' Store encrypted PIN
                StoreEncryptedPin(value)
            End If
        End Set
    End Property
    
    ' Auto-property with initializer
    Public Property AccountHolder As String = "Unknown"
    
    ' Computed property
    Public ReadOnly Property Age As TimeSpan
        Get
            Return DateTime.Now - _createdDate
        End Get
    End Property
    
    Public Sub Deposit(amount As Decimal)
        If amount > 0 Then
            Balance += amount
        End If
    End Sub
    
    Private Sub StoreEncryptedPin(pin As String)
        ' Implementation
    End Sub
End Class
```

## Methods

Methods define the behavior of objects:

```vb
Public Class Calculator
    ' Instance method
    Public Function Add(a As Double, b As Double) As Double
        Return a + b
    End Function
    
    ' Method with optional parameters
    Public Function Power(base As Double, Optional exponent As Double = 2) As Double
        Return Math.Pow(base, exponent)
    End Function
    
    ' Method with named parameters
    Public Sub PrintResult(value As Double, Optional prefix As String = "Result: ", Optional suffix As String = "")
        Console.WriteLine($"{prefix}{value}{suffix}")
    End Sub
    
    ' Method overloading
    Public Function Max(a As Integer, b As Integer) As Integer
        Return If(a > b, a, b)
    End Function
    
    Public Function Max(a As Double, b As Double) As Double
        Return If(a > b, a, b)
    End Function
    
    Public Function Max(ParamArray numbers() As Integer) As Integer
        Return numbers.Max()
    End Function
    
    ' Shared (static) method
    Public Shared Function Factorial(n As Integer) As Long
        If n <= 1 Then Return 1
        Return n * Factorial(n - 1)
    End Function
End Class

' Usage
Dim calc As New Calculator()
Console.WriteLine(calc.Add(5, 3))                          ' 8
Console.WriteLine(calc.Power(2))                           ' 4 (default exponent)
Console.WriteLine(calc.Power(2, 3))                        ' 8
calc.PrintResult(42, suffix:=" is the answer")             ' Result: 42 is the answer
Console.WriteLine(Calculator.Factorial(5))                 ' 120 (static method)
```

## Access Modifiers

VB.NET provides several access modifiers:

```vb
Public Class AccessDemo
    ' Public - accessible from anywhere
    Public PublicField As String = "Everyone can see this"
    
    ' Private - only within this class
    Private PrivateField As String = "Only this class"
    
    ' Protected - this class and derived classes
    Protected ProtectedField As String = "This class and children"
    
    ' Friend - anywhere in the same assembly
    Friend FriendField As String = "Same assembly only"
    
    ' Protected Friend - same assembly OR derived classes
    Protected Friend ProtectedFriendField As String = "Assembly or children"
    
    ' Private Protected - same assembly AND derived classes
    Private Protected PrivateProtectedField As String = "Assembly and children"
    
    ' Methods can also have access modifiers
    Public Sub PublicMethod()
        Console.WriteLine("Public method")
    End Sub
    
    Private Sub PrivateMethod()
        Console.WriteLine("Private method")
    End Sub
    
    Protected Overridable Sub ProtectedMethod()
        Console.WriteLine("Protected method")
    End Sub
End Class
```

## Shared (Static) Members

Shared members belong to the class itself, not to instances:

```vb
Public Class Statistics
    ' Shared fields
    Private Shared _count As Integer = 0
    Private Shared ReadOnly _startTime As DateTime = DateTime.Now
    
    ' Instance field
    Private _value As Double
    
    Public Sub New(value As Double)
        _value = value
        _count += 1  ' Increment shared counter
    End Sub
    
    ' Shared property
    Public Shared ReadOnly Property Count As Integer
        Get
            Return _count
        End Get
    End Property
    
    ' Shared method
    Public Shared Function Average(values() As Double) As Double
        If values.Length = 0 Then Return 0
        Return values.Average()
    End Function
    
    ' Shared events
    Public Shared Event ThresholdReached As EventHandler
    
    Public Shared Sub CheckThreshold()
        If _count > 100 Then
            RaiseEvent ThresholdReached(Nothing, EventArgs.Empty)
        End If
    End Sub
    
    ' Instance method accessing shared members
    Public Function GetInfo() As String
        Return $"Value: {_value}, Total instances: {_count}"
    End Function
End Class

' Usage
Dim stats1 As New Statistics(10)
Dim stats2 As New Statistics(20)
Console.WriteLine(Statistics.Count)  ' 2
Console.WriteLine(Statistics.Average({1, 2, 3, 4, 5}))  ' 3
```

## Inheritance

VB.NET supports single inheritance for classes:

```vb
' Base class
Public Class Vehicle
    Public Property Make As String
    Public Property Model As String
    Public Property Year As Integer
    
    Public Sub New(make As String, model As String, year As Integer)
        Me.Make = make
        Me.Model = model
        Me.Year = year
    End Sub
    
    Public Overridable Function GetInfo() As String
        Return $"{Year} {Make} {Model}"
    End Function
    
    Public Overridable Sub Start()
        Console.WriteLine("Vehicle starting...")
    End Sub
End Class

' Derived class
Public Class Car
    Inherits Vehicle  ' Inheritance
    
    Public Property NumberOfDoors As Integer
    
    ' Constructor calling base constructor
    Public Sub New(make As String, model As String, year As Integer, doors As Integer)
        MyBase.New(make, model, year)  ' Call base constructor
        NumberOfDoors = doors
    End Sub
    
    ' Override base method
    Public Overrides Function GetInfo() As String
        Return $"{MyBase.GetInfo()} ({NumberOfDoors} doors)"
    End Function
    
    ' Override with different implementation
    Public Overrides Sub Start()
        Console.WriteLine("Car engine starting...")
        MyBase.Start()  ' Optionally call base implementation
    End Sub
    
    ' New method specific to Car
    Public Sub OpenTrunk()
        Console.WriteLine("Trunk opened")
    End Sub
End Class

' Further inheritance
Public Class ElectricCar
    Inherits Car
    
    Public Property BatteryCapacity As Double
    
    Public Sub New(make As String, model As String, year As Integer, doors As Integer, battery As Double)
        MyBase.New(make, model, year, doors)
        BatteryCapacity = battery
    End Sub
    
    Public Overrides Sub Start()
        Console.WriteLine("Electric motor starting silently...")
    End Sub
    
    Public Sub Charge()
        Console.WriteLine($"Charging {BatteryCapacity} kWh battery")
    End Sub
End Class
```

## Abstract Classes

Abstract classes cannot be instantiated and may contain abstract members:

```vb
' Abstract base class
Public MustInherit Class Shape
    Public Property Name As String
    
    ' Abstract method - must be implemented by derived classes
    Public MustOverride Function CalculateArea() As Double
    Public MustOverride Function CalculatePerimeter() As Double
    
    ' Concrete method
    Public Function GetDescription() As String
        Return $"{Name}: Area = {CalculateArea():F2}, Perimeter = {CalculatePerimeter():F2}"
    End Function
End Class

' Concrete implementation
Public Class Circle
    Inherits Shape
    
    Public Property Radius As Double
    
    Public Sub New(radius As Double)
        Me.Radius = radius
        Name = "Circle"
    End Sub
    
    Public Overrides Function CalculateArea() As Double
        Return Math.PI * Radius * Radius
    End Function
    
    Public Overrides Function CalculatePerimeter() As Double
        Return 2 * Math.PI * Radius
    End Function
End Class

Public Class Rectangle
    Inherits Shape
    
    Public Property Width As Double
    Public Property Height As Double
    
    Public Sub New(width As Double, height As Double)
        Me.Width = width
        Me.Height = height
        Name = "Rectangle"
    End Sub
    
    Public Overrides Function CalculateArea() As Double
        Return Width * Height
    End Function
    
    Public Overrides Function CalculatePerimeter() As Double
        Return 2 * (Width + Height)
    End Function
End Class
```

## Interfaces

Interfaces define contracts that classes must implement:

```vb
' Interface definition
Public Interface IDrawable
    Sub Draw()
    Property Color As String
End Interface

Public Interface IResizable
    Sub Resize(factor As Double)
    ReadOnly Property CanResize As Boolean
End Interface

' Multiple interface implementation
Public Class GraphicShape
    Implements IDrawable, IResizable
    
    Private _color As String = "Black"
    
    Public Sub Draw() Implements IDrawable.Draw
        Console.WriteLine($"Drawing shape in {_color}")
    End Sub
    
    Public Property Color As String Implements IDrawable.Color
        Get
            Return _color
        End Get
        Set(value As String)
            _color = value
        End Set
    End Property
    
    Public Sub Resize(factor As Double) Implements IResizable.Resize
        Console.WriteLine($"Resizing by factor {factor}")
    End Sub
    
    Public ReadOnly Property CanResize As Boolean Implements IResizable.CanResize
        Get
            Return True
        End Get
    End Property
End Class

' Interface inheritance
Public Interface IShape
    Function GetArea() As Double
End Interface

Public Interface I3DShape
    Inherits IShape  ' Interface inheritance
    Function GetVolume() As Double
End Interface
```

## Polymorphism

Polymorphism allows objects to be treated as instances of their base type:

```vb
Public Class Zoo
    Private _animals As New List(Of Animal)
    
    Public Sub AddAnimal(animal As Animal)
        _animals.Add(animal)
    End Sub
    
    Public Sub FeedAllAnimals()
        For Each animal In _animals
            animal.Eat()  ' Polymorphic call
        Next
    End Sub
    
    Public Sub MakeAllSpeak()
        For Each animal In _animals
            Console.WriteLine(animal.Speak())  ' Polymorphic call
        Next
    End Sub
End Class

' Base class
Public MustInherit Class Animal
    Public Property Name As String
    
    Public Sub New(name As String)
        Me.Name = name
    End Sub
    
    Public MustOverride Function Speak() As String
    
    Public Overridable Sub Eat()
        Console.WriteLine($"{Name} is eating")
    End Sub
End Class

' Derived classes
Public Class Dog
    Inherits Animal
    
    Public Sub New(name As String)
        MyBase.New(name)
    End Sub
    
    Public Overrides Function Speak() As String
        Return $"{Name} says: Woof!"
    End Function
End Class

Public Class Cat
    Inherits Animal
    
    Public Sub New(name As String)
        MyBase.New(name)
    End Sub
    
    Public Overrides Function Speak() As String
        Return $"{Name} says: Meow!"
    End Function
    
    Public Overrides Sub Eat()
        Console.WriteLine($"{Name} is eating delicately")
    End Sub
End Class

' Usage
Dim zoo As New Zoo()
zoo.AddAnimal(New Dog("Rex"))
zoo.AddAnimal(New Cat("Whiskers"))
zoo.AddAnimal(New Dog("Buddy"))

zoo.MakeAllSpeak()  ' Each animal speaks differently
zoo.FeedAllAnimals()  ' Each animal eats (possibly differently)
```

## Partial Classes

Partial classes allow splitting a class definition across multiple files:

```vb
' File1.vb
Partial Public Class Customer
    Private _id As Integer
    Public Property Name As String
    
    Public Sub New(id As Integer, name As String)
        _id = id
        Me.Name = name
    End Sub
End Class

' File2.vb
Partial Public Class Customer
    Public Property Email As String
    Public Property Phone As String
    
    Public Function GetContactInfo() As String
        Return $"{Name}: {Email}, {Phone}"
    End Function
End Class
```

## Nested Classes

Classes can be defined within other classes:

```vb
Public Class Company
    Private _employees As New List(Of Employee)
    
    Public Property Name As String
    
    ' Nested class
    Public Class Employee
        Public Property Id As Integer
        Public Property Name As String
        Public Property Salary As Decimal
        
        Public Sub New(id As Integer, name As String, salary As Decimal)
            Me.Id = id
            Me.Name = name
            Me.Salary = salary
        End Sub
    End Class
    
    Public Sub AddEmployee(emp As Employee)
        _employees.Add(emp)
    End Sub
    
    Public Function GetTotalSalaries() As Decimal
        Return _employees.Sum(Function(e) e.Salary)
    End Function
End Class

' Usage
Dim company As New Company() With {.Name = "Tech Corp"}
Dim emp As New Company.Employee(1, "John", 50000)
company.AddEmployee(emp)
```

## Events and Delegates

Events allow objects to notify others when something happens:

```vb
Public Class Account
    Private _balance As Decimal
    
    ' Declare events
    Public Event Deposited(sender As Object, e As TransactionEventArgs)
    Public Event Withdrawn(sender As Object, e As TransactionEventArgs)
    Public Event OverdraftAttempted(sender As Object, e As TransactionEventArgs)
    
    Public Property AccountNumber As String
    
    Public Sub New(accountNumber As String)
        Me.AccountNumber = accountNumber
        _balance = 0
    End Sub
    
    Public Sub Deposit(amount As Decimal)
        If amount > 0 Then
            _balance += amount
            ' Raise event
            RaiseEvent Deposited(Me, New TransactionEventArgs(amount, _balance))
        End If
    End Sub
    
    Public Sub Withdraw(amount As Decimal)
        If amount > 0 AndAlso amount <= _balance Then
            _balance -= amount
            RaiseEvent Withdrawn(Me, New TransactionEventArgs(amount, _balance))
        ElseIf amount > _balance Then
            RaiseEvent OverdraftAttempted(Me, New TransactionEventArgs(amount, _balance))
        End If
    End Sub
End Class

' Event arguments class
Public Class TransactionEventArgs
    Inherits EventArgs
    
    Public ReadOnly Property Amount As Decimal
    Public ReadOnly Property NewBalance As Decimal
    
    Public Sub New(amount As Decimal, newBalance As Decimal)
        Me.Amount = amount
        Me.NewBalance = newBalance
    End Sub
End Class

' Usage with event handling
Public Class BankingApp
    Public Sub Run()
        Dim account As New Account("123456")
        
        ' Subscribe to events
        AddHandler account.Deposited, AddressOf OnDeposited
        AddHandler account.Withdrawn, AddressOf OnWithdrawn
        AddHandler account.OverdraftAttempted, AddressOf OnOverdraft
        
        account.Deposit(1000)
        account.Withdraw(200)
        account.Withdraw(1000)  ' Overdraft attempt
    End Sub
    
    Private Sub OnDeposited(sender As Object, e As TransactionEventArgs)
        Console.WriteLine($"Deposited: {e.Amount:C}, New Balance: {e.NewBalance:C}")
    End Sub
    
    Private Sub OnWithdrawn(sender As Object, e As TransactionEventArgs)
        Console.WriteLine($"Withdrawn: {e.Amount:C}, New Balance: {e.NewBalance:C}")
    End Sub
    
    Private Sub OnOverdraft(sender As Object, e As TransactionEventArgs)
        Console.WriteLine($"Overdraft attempted: {e.Amount:C}, Current Balance: {e.NewBalance:C}")
    End Sub
End Class
```

## Best Practices

1. **Encapsulation**: Keep fields private, expose through properties
2. **Single Responsibility**: Each class should have one primary purpose
3. **Meaningful Names**: Use descriptive names for classes, properties, and methods
4. **Avoid Deep Inheritance**: Prefer composition over deep inheritance hierarchies
5. **Use Interfaces**: Define contracts for better flexibility
6. **Immutability**: Consider making objects immutable when possible
7. **Dispose Pattern**: Implement IDisposable for resource cleanup

## Common Patterns

### Singleton Pattern

```vb
Public NotInheritable Class Logger
    Private Shared ReadOnly _instance As New Logger()
    Private _logFile As String
    
    Private Sub New()
        _logFile = "app.log"
    End Sub
    
    Public Shared ReadOnly Property Instance As Logger
        Get
            Return _instance
        End Get
    End Property
    
    Public Sub Log(message As String)
        File.AppendAllText(_logFile, $"{DateTime.Now}: {message}{Environment.NewLine}")
    End Sub
End Class

' Usage
Logger.Instance.Log("Application started")
```

### Factory Pattern

```vb
Public Interface IVehicle
    Sub Drive()
End Interface

Public Class Car
    Implements IVehicle
    
    Public Sub Drive() Implements IVehicle.Drive
        Console.WriteLine("Driving a car")
    End Sub
End Class

Public Class Truck
    Implements IVehicle
    
    Public Sub Drive() Implements IVehicle.Drive
        Console.WriteLine("Driving a truck")
    End Sub
End Class

Public Class VehicleFactory
    Public Shared Function CreateVehicle(type As String) As IVehicle
        Select Case type.ToLower()
            Case "car"
                Return New Car()
            Case "truck"
                Return New Truck()
            Case Else
                Throw New ArgumentException($"Unknown vehicle type: {type}")
        End Select
    End Function
End Class

' Usage
Dim vehicle = VehicleFactory.CreateVehicle("car")
vehicle.Drive()
```

## Summary

VB.NET's object-oriented features provide a robust foundation for building maintainable and scalable applications. Understanding classes, objects, inheritance, and polymorphism is essential for effective VB.NET programming. The language's clear syntax and powerful features make it an excellent choice for both beginners and experienced developers working on the .NET platform.