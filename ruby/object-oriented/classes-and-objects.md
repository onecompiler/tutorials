# Classes and Objects in Ruby

Ruby is a pure object-oriented language where everything is an object. Classes define the blueprint for objects, encapsulating data and behavior.

## Defining Classes

### Basic Class Definition

```ruby
class Person
  # Constructor
  def initialize(name, age)
    @name = name  # Instance variable
    @age = age
  end
  
  # Instance method
  def introduce
    "Hi, I'm #{@name} and I'm #{@age} years old."
  end
end

# Creating objects
person1 = Person.new("Alice", 30)
person2 = Person.new("Bob", 25)

puts person1.introduce  # Hi, I'm Alice and I'm 30 years old.
```

## Instance Variables and Methods

### Instance Variables

Instance variables start with `@` and are unique to each object:

```ruby
class BankAccount
  def initialize(account_number, initial_balance = 0)
    @account_number = account_number
    @balance = initial_balance
    @transactions = []
  end
  
  def deposit(amount)
    @balance += amount
    @transactions << { type: :deposit, amount: amount, time: Time.now }
    @balance
  end
  
  def withdraw(amount)
    if amount <= @balance
      @balance -= amount
      @transactions << { type: :withdrawal, amount: amount, time: Time.now }
      @balance
    else
      raise "Insufficient funds"
    end
  end
  
  def balance
    @balance
  end
end

account = BankAccount.new("12345", 1000)
account.deposit(500)
puts account.balance  # 1500
```

## Getters and Setters

### Manual Getters and Setters

```ruby
class Product
  def initialize(name, price)
    @name = name
    @price = price
  end
  
  # Getter methods
  def name
    @name
  end
  
  def price
    @price
  end
  
  # Setter methods
  def name=(new_name)
    @name = new_name
  end
  
  def price=(new_price)
    @price = new_price if new_price > 0
  end
end
```

### Using attr_accessor, attr_reader, attr_writer

```ruby
class Employee
  attr_reader :id, :hire_date      # Creates getter methods
  attr_writer :department           # Creates setter method
  attr_accessor :name, :position    # Creates both getter and setter
  
  def initialize(id, name)
    @id = id
    @name = name
    @hire_date = Date.today
  end
end

emp = Employee.new(1, "John")
puts emp.name          # John (getter)
emp.name = "John Doe"  # (setter)
emp.department = "IT"  # (setter only)
# emp.id = 2          # Error! No setter for id
```

## Class Variables and Methods

### Class Variables

Class variables are shared among all instances of a class:

```ruby
class User
  @@user_count = 0  # Class variable
  
  def initialize(username)
    @username = username
    @@user_count += 1
    @user_id = @@user_count
  end
  
  def self.total_users  # Class method
    @@user_count
  end
  
  def info
    "User ##{@user_id}: #{@username}"
  end
end

user1 = User.new("alice")
user2 = User.new("bob")
user3 = User.new("charlie")

puts User.total_users  # 3
puts user2.info       # User #2: bob
```

### Class Methods

```ruby
class MathHelper
  # Class method using self
  def self.circle_area(radius)
    Math::PI * radius ** 2
  end
  
  # Alternative syntax
  class << self
    def rectangle_area(length, width)
      length * width
    end
    
    def triangle_area(base, height)
      0.5 * base * height
    end
  end
end

puts MathHelper.circle_area(5)      # 78.53981633974483
puts MathHelper.rectangle_area(4, 6) # 24
```

## Inheritance

Ruby supports single inheritance with the ability to include modules for multiple inheritance-like behavior:

```ruby
# Base class
class Animal
  attr_reader :name
  
  def initialize(name)
    @name = name
  end
  
  def speak
    "#{@name} makes a sound"
  end
  
  def move
    "#{@name} moves"
  end
end

# Derived classes
class Dog < Animal
  def initialize(name, breed)
    super(name)  # Call parent constructor
    @breed = breed
  end
  
  def speak
    "#{@name} barks: Woof!"
  end
  
  def fetch
    "#{@name} fetches the ball"
  end
end

class Cat < Animal
  def speak
    "#{@name} meows: Meow!"
  end
  
  def scratch
    "#{@name} scratches"
  end
end

dog = Dog.new("Buddy", "Golden Retriever")
cat = Cat.new("Whiskers")

puts dog.speak   # Buddy barks: Woof!
puts dog.move    # Buddy moves (inherited)
puts cat.speak   # Whiskers meows: Meow!
```

## Method Visibility

Ruby provides three levels of method visibility:

```ruby
class SecureData
  def initialize(data)
    @data = data
  end
  
  # Public method (default)
  def display_data
    "Data: #{process_data}"
  end
  
  private  # Everything below is private
  
  def process_data
    encrypt_data
    format_data
  end
  
  def encrypt_data
    @data.reverse  # Simple "encryption"
  end
  
  def format_data
    @data.upcase
  end
  
  protected  # Everything below is protected
  
  def internal_id
    object_id
  end
end

# Usage
secure = SecureData.new("secret")
puts secure.display_data  # Works
# secure.process_data     # Error! Private method
```

### Private vs Protected

```ruby
class Person
  def initialize(name, age)
    @name = name
    @age = age
  end
  
  def older_than?(other_person)
    self.age > other_person.age  # Can access protected method
  end
  
  protected
  
  def age
    @age
  end
  
  private
  
  def secret
    "This is private"
  end
end

alice = Person.new("Alice", 30)
bob = Person.new("Bob", 25)
puts alice.older_than?(bob)  # true
# alice.age                  # Error! Protected method
```

## Constants

Constants in Ruby start with uppercase letters:

```ruby
class Configuration
  VERSION = "1.0.0"
  MAX_USERS = 1000
  DEFAULT_SETTINGS = {
    theme: "dark",
    language: "en"
  }.freeze
  
  class Database
    HOST = "localhost"
    PORT = 5432
  end
end

puts Configuration::VERSION           # 1.0.0
puts Configuration::Database::HOST    # localhost

# Constants can be changed (with warning)
Configuration::VERSION = "1.0.1"  # Warning: already initialized constant
```

## Method Overriding and Super

```ruby
class Vehicle
  def initialize(make, model)
    @make = make
    @model = model
  end
  
  def info
    "#{@make} #{@model}"
  end
  
  def start
    check_fuel
    "Vehicle starting..."
  end
  
  private
  
  def check_fuel
    puts "Checking fuel..."
  end
end

class ElectricCar < Vehicle
  def initialize(make, model, battery_capacity)
    super(make, model)  # Call parent with specific args
    @battery_capacity = battery_capacity
  end
  
  def info
    super + " (Electric, #{@battery_capacity}kWh)"  # Extend parent method
  end
  
  def start
    check_battery
    super  # Call parent method
  end
  
  private
  
  def check_battery
    puts "Checking battery..."
  end
end

tesla = ElectricCar.new("Tesla", "Model 3", 75)
puts tesla.info   # Tesla Model 3 (Electric, 75kWh)
puts tesla.start  # Checking battery... Checking fuel... Vehicle starting...
```

## Class Inheritance Chain

```ruby
class Animal; end
class Mammal < Animal; end
class Dog < Mammal; end

dog = Dog.new

# Check inheritance
puts dog.is_a?(Dog)      # true
puts dog.is_a?(Mammal)   # true
puts dog.is_a?(Animal)   # true
puts dog.is_a?(Object)   # true (everything inherits from Object)

# Class hierarchy
puts Dog.ancestors
# [Dog, Mammal, Animal, Object, Kernel, BasicObject]

# Superclass
puts Dog.superclass      # Mammal
puts Mammal.superclass   # Animal
puts Animal.superclass   # Object
```

## Singleton Methods

Methods defined on specific objects:

```ruby
class Car
  def initialize(model)
    @model = model
  end
end

car1 = Car.new("Toyota")
car2 = Car.new("Honda")

# Define method only for car1
def car1.turbo_boost
  "#{@model} activates turbo!"
end

puts car1.turbo_boost  # Toyota activates turbo!
# car2.turbo_boost     # Error! Method doesn't exist

# Alternative syntax
car2.define_singleton_method(:eco_mode) do
  "#{@model} switches to eco mode"
end

puts car2.eco_mode     # Honda switches to eco mode
```

## Duck Typing

Ruby follows the principle of duck typing - "If it walks like a duck and quacks like a duck, it's a duck":

```ruby
class FileLogger
  def log(message)
    File.open("app.log", "a") { |f| f.puts "[FILE] #{message}" }
  end
end

class ConsoleLogger
  def log(message)
    puts "[CONSOLE] #{message}"
  end
end

class EmailLogger
  def log(message)
    # Simulate sending email
    puts "[EMAIL] Sending: #{message}"
  end
end

# All three can be used interchangeably
def process_data(logger)
  logger.log("Processing started")
  # ... do work ...
  logger.log("Processing completed")
end

process_data(FileLogger.new)
process_data(ConsoleLogger.new)
process_data(EmailLogger.new)
```

## Best Practices

1. **Single Responsibility**: Each class should have one reason to change
2. **Descriptive Names**: Use clear, meaningful class and method names
3. **Avoid Deep Inheritance**: Prefer composition over inheritance
4. **Use Modules**: For shared behavior across unrelated classes
5. **Initialize Properly**: Set all instance variables in initialize
6. **Consistent Interfaces**: Methods should behave predictably
7. **Encapsulation**: Keep internal details private

## Common Patterns

### Factory Pattern

```ruby
class AnimalFactory
  def self.create(type, name)
    case type
    when :dog then Dog.new(name)
    when :cat then Cat.new(name)
    when :bird then Bird.new(name)
    else raise "Unknown animal type"
    end
  end
end

pet = AnimalFactory.create(:dog, "Rex")
```

### Builder Pattern

```ruby
class Computer
  attr_accessor :cpu, :ram, :storage
  
  class Builder
    def initialize
      @computer = Computer.new
    end
    
    def with_cpu(cpu)
      @computer.cpu = cpu
      self
    end
    
    def with_ram(ram)
      @computer.ram = ram
      self
    end
    
    def with_storage(storage)
      @computer.storage = storage
      self
    end
    
    def build
      @computer
    end
  end
end

computer = Computer::Builder.new
  .with_cpu("Intel i7")
  .with_ram("16GB")
  .with_storage("512GB SSD")
  .build
```