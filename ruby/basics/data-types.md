# Data Types in Ruby

Ruby is dynamically typed - variables don't have types, but the objects they reference do. Ruby provides several built-in data types, and everything in Ruby is an object.

## 1. Numbers

Ruby has several numeric classes for different types of numbers:

### Integer

In Ruby 2.4+, Fixnum and Bignum were unified into Integer:

```ruby
# Integers
age = 25
year = 2024
negative = -100
big_number = 999_999_999_999  # Underscores for readability

# Integer methods
puts 10.class          # Integer
puts 10.even?          # true
puts 15.odd?           # true
puts -5.abs            # 5
puts 10.times { |i| print "#{i} " }  # 0 1 2 3 4 5 6 7 8 9
```

### Float

Floating-point numbers:

```ruby
price = 19.99
pi = 3.14159
scientific = 1.5e-3  # 0.0015

# Float methods
puts price.round(1)    # 20.0
puts pi.ceil          # 4
puts pi.floor         # 3
puts (10.0 / 3).round(2)  # 3.33
```

### Rational

Represents fractions precisely:

```ruby
# Creating rationals
r1 = Rational(3, 4)    # 3/4
r2 = 0.75.to_r         # 3/4
r3 = Rational('2/3')   # 2/3

# Rational arithmetic is precise
puts Rational(1, 3) + Rational(1, 3)  # 2/3
puts Rational(1, 2) * 4               # 2/1

# Convert to float
puts Rational(2, 3).to_f              # 0.6666666666666666
```

### Complex

For complex numbers:

```ruby
# Creating complex numbers
c1 = Complex(3, 4)     # 3+4i
c2 = 3 + 4i           # 3+4i (Ruby 2.1+)

# Complex operations
puts c1 + c2          # 6+8i
puts c1.abs           # 5.0 (magnitude)
puts c1.real          # 3
puts c1.imaginary     # 4
```

### BigDecimal

For arbitrary precision decimal arithmetic:

```ruby
require 'bigdecimal'

# Creating BigDecimal
price = BigDecimal("19.99")
tax_rate = BigDecimal("0.08")

# Precise calculations
total = price * (1 + tax_rate)
puts total.to_s('F')  # 21.5892

# Rounding
puts total.round(2).to_s('F')  # 21.59
```

## 2. Boolean

Ruby has two boolean values: `true` and `false`. Ruby also has `nil`, which represents "nothing" or "no value".

```ruby
is_active = true
is_deleted = false
result = nil

# Truthy and Falsy
# In Ruby, only false and nil are falsy. Everything else is truthy!
puts "0 is truthy" if 0          # This prints!
puts "Empty string is truthy" if ""  # This prints!
puts "Empty array is truthy" if []   # This prints!

# Boolean methods
puts true.class   # TrueClass
puts false.class  # FalseClass
puts nil.class    # NilClass

# Checking for nil
value = nil
puts value.nil?   # true
puts value.to_s   # "" (empty string)
```

## 3. Strings

Strings are sequences of characters. Ruby provides extensive string manipulation methods.

```ruby
# Creating strings
single = 'Hello'
double = "World"
multiline = <<~TEXT
  This is a
  multiline string
  with proper indentation
TEXT

# String interpolation (only in double quotes)
name = "Ruby"
greeting = "Hello, #{name}!"  # "Hello, Ruby!"
calculation = "2 + 2 = #{2 + 2}"  # "2 + 2 = 4"

# String methods
text = "ruby programming"
puts text.length           # 16
puts text.upcase          # RUBY PROGRAMMING
puts text.capitalize      # Ruby programming
puts text.reverse         # gnimmargorp ybur
puts text.include?("ruby") # true
puts text.split           # ["ruby", "programming"]
puts text[0..3]           # ruby (substring)
puts text.gsub("ruby", "Ruby")  # Ruby programming

# String concatenation
str1 = "Hello"
str2 = "World"
puts str1 + " " + str2    # Hello World
puts "#{str1} #{str2}"    # Hello World (interpolation)
puts str1 << " " << str2  # Hello World (modifies str1)

# Escape sequences
puts "Line 1\nLine 2"     # New line
puts "Tab\there"          # Tab
puts "Quote: \"Hello\""   # Escaped quotes
```

## 4. Symbols

Symbols are immutable, reusable identifiers. They're perfect for hash keys and constants.

```ruby
# Creating symbols
status = :active
method_name = :to_s
hash_key = :user_id

# Symbols vs Strings
puts :ruby.object_id      # Same object ID every time
puts :ruby.object_id      # Same!
puts "ruby".object_id     # Different object ID
puts "ruby".object_id     # Different!

# Symbol methods
puts :hello.to_s          # "hello"
puts "hello".to_sym       # :hello
puts :hello.class         # Symbol
puts :hello.upcase        # :HELLO

# Common usage
user = {
  :name => "John",        # Traditional syntax
  age: 25,                # Modern syntax (Ruby 1.9+)
  :email => "john@example.com"
}
```

## 5. Arrays

Arrays are ordered collections that can hold any type of object.

```ruby
# Creating arrays
numbers = [1, 2, 3, 4, 5]
mixed = ["hello", 42, true, 3.14, :symbol]
nested = [[1, 2], [3, 4], [5, 6]]
empty = []
words = %w[apple banana cherry]  # ["apple", "banana", "cherry"]

# Accessing elements
arr = [10, 20, 30, 40, 50]
puts arr[0]        # 10 (first element)
puts arr[-1]       # 50 (last element)
puts arr[1..3]     # [20, 30, 40] (range)
puts arr.first     # 10
puts arr.last      # 50
puts arr.first(2)  # [10, 20]

# Array methods
arr = [3, 1, 4, 1, 5]
puts arr.length       # 5
puts arr.sort         # [1, 1, 3, 4, 5]
puts arr.uniq         # [3, 1, 4, 5]
puts arr.reverse      # [5, 1, 4, 1, 3]
puts arr.include?(4)  # true
puts arr.push(9)      # [3, 1, 4, 1, 5, 9]
puts arr.pop          # 9 (removes and returns)
puts arr.join("-")    # "3-1-4-1-5"

# Array iteration
[1, 2, 3].each { |n| puts n * 2 }
[1, 2, 3].map { |n| n * 2 }  # [2, 4, 6]
[1, 2, 3, 4, 5].select { |n| n.even? }  # [2, 4]
```

## 6. Hashes

Hashes are collections of key-value pairs, similar to dictionaries in other languages.

```ruby
# Creating hashes
# Old syntax
old_hash = { "name" => "Ruby", "year" => 1995 }

# Modern syntax (for symbol keys)
person = { name: "Alice", age: 30, city: "NYC" }

# Mixed keys
mixed = { :symbol => "value", "string" => 123, 42 => "number key" }

# Accessing values
user = { name: "Bob", email: "bob@example.com", age: 25 }
puts user[:name]      # Bob
puts user["name"]     # nil (symbol != string)
puts user.fetch(:age) # 25
puts user.fetch(:phone, "N/A")  # N/A (default value)

# Hash methods
person = { name: "Charlie", age: 35 }
puts person.keys      # [:name, :age]
puts person.values    # ["Charlie", 35]
puts person.has_key?(:name)  # true
puts person.empty?    # false
puts person.size      # 2

# Modifying hashes
person[:city] = "Boston"     # Add/update
person.delete(:age)          # Remove
person.merge!(job: "Developer")  # Merge another hash

# Hash iteration
person.each do |key, value|
  puts "#{key}: #{value}"
end

# Default values
scores = Hash.new(0)  # Default value is 0
scores[:math] += 10   # Works even though :math doesn't exist yet
puts scores[:math]    # 10
```

## 7. Ranges

Ranges represent intervals of values.

```ruby
# Creating ranges
numbers = 1..10      # Inclusive: 1, 2, 3...10
letters = 'a'..'z'   # a, b, c...z
exclusive = 1...10   # Exclusive: 1, 2, 3...9 (not 10)

# Range methods
range = 1..5
puts range.include?(3)   # true
puts range.min          # 1
puts range.max          # 5
puts range.to_a         # [1, 2, 3, 4, 5]

# Using ranges
(1..5).each { |n| puts n }
case score
when 90..100 then "A"
when 80..89 then "B"
when 70..79 then "C"
end
```

## Type Conversion

Ruby provides methods to convert between types:

```ruby
# To String
puts 42.to_s          # "42"
puts [1, 2, 3].to_s   # "[1, 2, 3]"
puts :symbol.to_s     # "symbol"

# To Integer
puts "42".to_i        # 42
puts "42.8".to_i      # 42
puts "abc".to_i       # 0

# To Float
puts "3.14".to_f      # 3.14
puts 42.to_f          # 42.0

# To Array
puts (1..5).to_a      # [1, 2, 3, 4, 5]
puts "hello".chars    # ["h", "e", "l", "l", "o"]

# To Symbol
puts "method_name".to_sym  # :method_name
```

## Checking Types

```ruby
# Using class method
puts 42.class         # Integer
puts "hello".class    # String
puts [].class         # Array

# Using is_a? and kind_of?
puts 42.is_a?(Integer)     # true
puts 42.is_a?(Numeric)     # true (Integer is a Numeric)
puts "hello".kind_of?(String)  # true

# Using respond_to?
puts "hello".respond_to?(:upcase)  # true
puts 42.respond_to?(:upcase)       # false
```

