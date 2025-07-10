# Modules and Mixins in Ruby

Modules are Ruby's way of grouping related methods, classes, and constants together. They serve two main purposes: namespacing and mixins (multiple inheritance).

## Creating Modules

### Basic Module Definition

```ruby
module Greetings
  LANGUAGES = ['English', 'Spanish', 'French', 'Japanese']
  
  def say_hello
    "Hello!"
  end
  
  def say_goodbye
    "Goodbye!"
  end
  
  # Module method
  def self.available_languages
    LANGUAGES.join(", ")
  end
end

# Access module constant
puts Greetings::LANGUAGES  # ["English", "Spanish", "French", "Japanese"]

# Call module method
puts Greetings.available_languages  # English, Spanish, French, Japanese
```

## Modules as Mixins

Mixins allow you to share functionality between classes without inheritance:

```ruby
module Printable
  def print_info
    puts "Printing #{self.class} information:"
    puts self.to_s
  end
  
  def print_json
    require 'json'
    puts self.to_h.to_json
  end
end

class Product
  include Printable
  
  attr_accessor :name, :price
  
  def initialize(name, price)
    @name = name
    @price = price
  end
  
  def to_s
    "#{@name}: $#{@price}"
  end
  
  def to_h
    { name: @name, price: @price }
  end
end

product = Product.new("Laptop", 999.99)
product.print_info   # Printing Product information:
                    # Laptop: $999.99
product.print_json  # {"name":"Laptop","price":999.99}
```

## Include vs Extend vs Prepend

### Include (Instance Methods)

`include` adds module methods as instance methods:

```ruby
module Walkable
  def walk
    "#{self.class.name} is walking"
  end
end

class Person
  include Walkable
end

person = Person.new
puts person.walk  # Person is walking
```

### Extend (Class Methods)

`extend` adds module methods as class methods:

```ruby
module ClassUtilities
  def description
    "This is the #{self.name} class"
  end
  
  def instance_count
    @instance_count ||= 0
  end
  
  def increment_count
    @instance_count = instance_count + 1
  end
end

class Animal
  extend ClassUtilities
  
  def initialize
    self.class.increment_count
  end
end

puts Animal.description     # This is the Animal class
Animal.new
Animal.new
puts Animal.instance_count  # 2
```

### Prepend (Method Override)

`prepend` inserts the module before the class in the method lookup chain:

```ruby
module Timestamped
  def save
    @updated_at = Time.now
    puts "Setting timestamp: #{@updated_at}"
    super  # Call the original save method
  end
end

class Document
  prepend Timestamped
  
  def save
    puts "Saving document..."
    true
  end
end

doc = Document.new
doc.save
# Output:
# Setting timestamp: 2024-01-10 10:30:45
# Saving document...
```

## Multiple Mixins

A class can include multiple modules:

```ruby
module Comparable
  def between?(min, max)
    self >= min && self <= max
  end
end

module Printable
  def display
    puts self.inspect
  end
end

module Trackable
  def track_change(attribute, old_value, new_value)
    @changes ||= []
    @changes << {
      attribute: attribute,
      old: old_value,
      new: new_value,
      timestamp: Time.now
    }
  end
  
  def changes
    @changes || []
  end
end

class Product
  include Comparable
  include Printable
  include Trackable
  
  attr_reader :price
  
  def initialize(name, price)
    @name = name
    @price = price
  end
  
  def price=(new_price)
    track_change(:price, @price, new_price)
    @price = new_price
  end
  
  def <=>(other)
    self.price <=> other.price
  end
end

p1 = Product.new("Book", 15.99)
p2 = Product.new("Pen", 2.99)

puts p1 > p2           # true
puts p2.between?(p1, p1)  # false
p1.display             # #<Product:0x... @name="Book", @price=15.99>

p1.price = 12.99
puts p1.changes        # [{:attribute=>:price, :old=>15.99, :new=>12.99, ...}]
```

## Module Namespacing

Modules can be used to organize code and prevent naming conflicts:

```ruby
module Store
  class Product
    def initialize(name)
      @name = name
      @store_type = "Retail"
    end
  end
  
  module Inventory
    class Product
      def initialize(sku)
        @sku = sku
        @location = "Warehouse"
      end
    end
    
    class Manager
      def check_stock(sku)
        "Checking stock for #{sku}"
      end
    end
  end
end

# Different Product classes
retail_product = Store::Product.new("Shirt")
inventory_product = Store::Inventory::Product.new("SKU123")
manager = Store::Inventory::Manager.new
```

## Module Methods

Several ways to define module methods:

```ruby
module MathHelpers
  # Method 1: Using self
  def self.square(n)
    n ** 2
  end
  
  # Method 2: Using module_function
  module_function
  
  def cube(n)
    n ** 3
  end
  
  def power(base, exp)
    base ** exp
  end
  
  # Method 3: Using extend self
  module StringHelpers
    extend self
    
    def titleize(str)
      str.split.map(&:capitalize).join(' ')
    end
    
    def underscore(str)
      str.gsub(/([A-Z])/, '_\1').downcase.strip.delete_prefix('_')
    end
  end
end

# Usage
puts MathHelpers.square(5)      # 25
puts MathHelpers.cube(3)        # 27
puts MathHelpers::StringHelpers.titleize("hello world")  # Hello World
```

## Module Callbacks

Modules can hook into the inclusion process:

```ruby
module Auditable
  def self.included(base)
    base.extend(ClassMethods)
    base.class_eval do
      attr_accessor :created_at, :updated_at
    end
  end
  
  module ClassMethods
    def with_timestamps
      puts "Timestamps enabled for #{self.name}"
    end
  end
  
  def touch
    @updated_at = Time.now
  end
  
  def fresh?
    return false unless @updated_at
    Time.now - @updated_at < 3600  # Less than 1 hour old
  end
end

class Article
  include Auditable
  
  def initialize(title)
    @title = title
    @created_at = Time.now
    @updated_at = Time.now
  end
end

Article.with_timestamps  # Timestamps enabled for Article
article = Article.new("Ruby Modules")
puts article.fresh?      # true
```

## Enumerable Module

One of Ruby's most powerful mixins:

```ruby
class TodoList
  include Enumerable
  
  def initialize
    @items = []
  end
  
  def add(item)
    @items << item
  end
  
  # Must define 'each' for Enumerable
  def each
    @items.each { |item| yield(item) }
  end
end

list = TodoList.new
list.add("Buy milk")
list.add("Walk dog")
list.add("Write code")

# Now we can use Enumerable methods
puts list.map(&:upcase)         # ["BUY MILK", "WALK DOG", "WRITE CODE"]
puts list.select { |i| i.include?("o") }  # ["Walk dog", "Write code"]
puts list.first                 # Buy milk
puts list.count                 # 3
```

## Comparable Module

For objects that can be ordered:

```ruby
class Version
  include Comparable
  
  attr_reader :major, :minor, :patch
  
  def initialize(version_string)
    @major, @minor, @patch = version_string.split('.').map(&:to_i)
  end
  
  # Must define <=> for Comparable
  def <=>(other)
    return nil unless other.is_a?(Version)
    
    [major, minor, patch] <=> [other.major, other.minor, other.patch]
  end
  
  def to_s
    "#{major}.#{minor}.#{patch}"
  end
end

v1 = Version.new("2.1.0")
v2 = Version.new("2.0.5")
v3 = Version.new("2.1.0")

puts v1 > v2          # true
puts v1 == v3         # true
puts v2.between?(v1, v3)  # false
puts [v1, v2, v3].sort.map(&:to_s)  # ["2.0.5", "2.1.0", "2.1.0"]
```

## Method Lookup Chain

Understanding how Ruby finds methods:

```ruby
module A
  def hello
    "Hello from A"
  end
end

module B
  def hello
    "Hello from B"
  end
end

class MyClass
  include A
  include B  # B is included last, so it takes precedence
end

obj = MyClass.new
puts obj.hello  # "Hello from B"

# Check method lookup chain
puts MyClass.ancestors
# [MyClass, B, A, Object, Kernel, BasicObject]
```

## Real-World Module Examples

### Authentication Module

```ruby
module Authenticatable
  def self.included(base)
    base.extend(ClassMethods)
    base.attr_accessor :password_digest
  end
  
  module ClassMethods
    def authenticate(email, password)
      user = find_by_email(email)
      return nil unless user
      user.valid_password?(password) ? user : nil
    end
  end
  
  def password=(new_password)
    # In real app, use BCrypt
    @password_digest = simple_hash(new_password)
  end
  
  def valid_password?(password)
    @password_digest == simple_hash(password)
  end
  
  private
  
  def simple_hash(string)
    string.reverse  # Don't use in production!
  end
end

class User
  include Authenticatable
  
  attr_accessor :email
  
  def initialize(email, password)
    @email = email
    self.password = password
  end
  
  def self.find_by_email(email)
    # Simulate database lookup
    nil
  end
end
```

### Validation Module

```ruby
module Validatable
  def self.included(base)
    base.extend(ClassMethods)
  end
  
  module ClassMethods
    def validates_presence_of(*attributes)
      @validations ||= []
      @validations << { type: :presence, attributes: attributes }
    end
    
    def validations
      @validations || []
    end
  end
  
  def valid?
    self.class.validations.all? do |validation|
      case validation[:type]
      when :presence
        validation[:attributes].all? do |attr|
          value = send(attr)
          !value.nil? && !value.to_s.strip.empty?
        end
      end
    end
  end
  
  def errors
    errors = []
    self.class.validations.each do |validation|
      validation[:attributes].each do |attr|
        value = send(attr)
        if value.nil? || value.to_s.strip.empty?
          errors << "#{attr} can't be blank"
        end
      end
    end
    errors
  end
end

class Product
  include Validatable
  
  attr_accessor :name, :price
  
  validates_presence_of :name, :price
  
  def initialize(name = nil, price = nil)
    @name = name
    @price = price
  end
end

product = Product.new
puts product.valid?  # false
puts product.errors  # ["name can't be blank", "price can't be blank"]
```

## Best Practices

1. **Single Responsibility**: Modules should have a focused purpose
2. **Clear Naming**: Use descriptive names ending in '-able' for mixins
3. **Avoid State**: Modules should primarily contain behavior, not state
4. **Document Dependencies**: Clearly state what methods a module expects
5. **Use Namespacing**: Prevent naming conflicts with module namespaces
6. **Consider Composition**: Sometimes object composition is clearer than mixins

## Common Pitfalls

```ruby
# Pitfall 1: Module state shared between instances
module Counter
  def increment
    @count ||= 0
    @count += 1
  end
end

# Better: Let the including class manage state
module CounterBehavior
  def increment
    self.count = (count || 0) + 1
  end
end

# Pitfall 2: Name conflicts
module A
  def name; "A"; end
end

module B
  def name; "B"; end
end

class C
  include A
  include B  # B's name method wins
end

# Solution: Use aliasing or prepend
module A
  def name; "A:" + super; end
end
```