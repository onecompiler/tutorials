# Arrays in Ruby

Arrays are ordered, integer-indexed collections of any object. They are one of Ruby's most fundamental data structures and provide powerful methods for data manipulation.

## Creating Arrays

### Basic Array Creation

```ruby
# Empty array
empty = []
empty = Array.new

# Array with initial values
numbers = [1, 2, 3, 4, 5]
mixed = ["hello", 42, true, 3.14, :symbol, nil]
nested = [[1, 2], [3, 4], [5, 6]]

# Array with size and default value
zeros = Array.new(5, 0)  # [0, 0, 0, 0, 0]

# Array with block initialization
squares = Array.new(5) { |i| i * i }  # [0, 1, 4, 9, 16]

# Using %w and %i shortcuts
words = %w[apple banana cherry]      # ["apple", "banana", "cherry"]
symbols = %i[red green blue]          # [:red, :green, :blue]
```

## Accessing Array Elements

```ruby
arr = ["a", "b", "c", "d", "e"]

# Positive indexing (0-based)
puts arr[0]      # "a" (first element)
puts arr[2]      # "c" (third element)

# Negative indexing
puts arr[-1]     # "e" (last element)
puts arr[-2]     # "d" (second to last)

# Range access
puts arr[1..3]   # ["b", "c", "d"]
puts arr[1...3]  # ["b", "c"] (exclusive)
puts arr[1, 3]   # ["b", "c", "d"] (start at 1, take 3)

# Methods for accessing
puts arr.first      # "a"
puts arr.last       # "e"
puts arr.first(2)   # ["a", "b"]
puts arr.last(3)    # ["c", "d", "e"]
puts arr.fetch(10, "default")  # "default" (safe access)
puts arr.sample     # Random element
```

## Modifying Arrays

### Adding Elements

```ruby
arr = [1, 2, 3]

# Push (add to end)
arr.push(4)          # [1, 2, 3, 4]
arr << 5             # [1, 2, 3, 4, 5]
arr.append(6)        # [1, 2, 3, 4, 5, 6]

# Unshift (add to beginning)
arr.unshift(0)       # [0, 1, 2, 3, 4, 5, 6]

# Insert at specific position
arr.insert(3, 2.5)   # [0, 1, 2, 2.5, 3, 4, 5, 6]

# Concatenation
arr1 = [1, 2]
arr2 = [3, 4]
arr3 = arr1 + arr2   # [1, 2, 3, 4]
arr1.concat(arr2)    # Modifies arr1: [1, 2, 3, 4]
```

### Removing Elements

```ruby
arr = [1, 2, 3, 4, 5, 3]

# Pop (remove from end)
last = arr.pop       # Returns 3, arr = [1, 2, 3, 4, 5]

# Shift (remove from beginning)
first = arr.shift    # Returns 1, arr = [2, 3, 4, 5]

# Delete specific value (all occurrences)
arr.delete(3)        # Removes all 3s

# Delete at index
arr.delete_at(1)     # Removes element at index 1

# Compact (remove nil values)
[1, nil, 2, nil, 3].compact  # [1, 2, 3]

# Clear all elements
arr.clear            # []
```

## Array Iteration

### Basic Iteration

```ruby
fruits = ["apple", "banana", "cherry"]

# Each
fruits.each do |fruit|
  puts fruit.upcase
end

# Each with index
fruits.each_with_index do |fruit, index|
  puts "#{index}: #{fruit}"
end

# Reverse each
fruits.reverse_each { |fruit| puts fruit }

# Each slice
[1, 2, 3, 4, 5, 6].each_slice(2) do |pair|
  puts pair.inspect  # [1, 2], [3, 4], [5, 6]
end
```

### Transformation Methods

```ruby
numbers = [1, 2, 3, 4, 5]

# Map (creates new array)
squares = numbers.map { |n| n ** 2 }  # [1, 4, 9, 16, 25]

# Map! (modifies original)
numbers.map! { |n| n * 2 }  # numbers = [2, 4, 6, 8, 10]

# Select (filter)
evens = numbers.select { |n| n.even? }  # [2, 4, 6, 8, 10]
odds = numbers.reject { |n| n.even? }   # []

# Reduce/Inject
sum = [1, 2, 3, 4].reduce(:+)      # 10
product = [1, 2, 3, 4].reduce(:*)  # 24
sum = [1, 2, 3, 4].reduce(10) { |acc, n| acc + n }  # 20 (with initial value)

# Partition
nums = [1, 2, 3, 4, 5, 6]
evens, odds = nums.partition(&:even?)  # [[2, 4, 6], [1, 3, 5]]
```

## Array Operations

### Searching and Testing

```ruby
arr = [1, 2, 3, 4, 5]

# Include?
puts arr.include?(3)     # true

# Any? / All? / None?
puts arr.any? { |n| n > 3 }    # true
puts arr.all? { |n| n > 0 }    # true
puts arr.none? { |n| n < 0 }   # true

# Find / Detect
first_even = arr.find { |n| n.even? }  # 2
all_evens = arr.find_all { |n| n.even? }  # [2, 4]

# Count
puts arr.count              # 5
puts arr.count(3)          # 1 (count occurrences)
puts arr.count { |n| n.even? }  # 2

# Index / Rindex
puts arr.index(3)          # 2
puts arr.rindex(3)         # 2 (search from end)
```

### Sorting and Ordering

```ruby
# Sort
numbers = [3, 1, 4, 1, 5, 9]
sorted = numbers.sort           # [1, 1, 3, 4, 5, 9]
numbers.sort!                   # Modifies original

# Sort with block
words = ["apple", "pie", "zoo", "banana"]
by_length = words.sort { |a, b| a.length <=> b.length }
by_reverse = words.sort { |a, b| b <=> a }  # Descending

# Sort_by
people = [{name: "Alice", age: 30}, {name: "Bob", age: 25}]
by_age = people.sort_by { |person| person[:age] }

# Reverse
arr = [1, 2, 3]
reversed = arr.reverse      # [3, 2, 1]
arr.reverse!               # Modifies original

# Shuffle
shuffled = arr.shuffle     # Random order
```

### Set Operations

```ruby
a = [1, 2, 3, 4]
b = [3, 4, 5, 6]

# Union
puts a | b          # [1, 2, 3, 4, 5, 6]

# Intersection
puts a & b          # [3, 4]

# Difference
puts a - b          # [1, 2]

# Unique elements
arr = [1, 1, 2, 2, 3, 3]
puts arr.uniq       # [1, 2, 3]
arr.uniq!          # Modifies original
```

## Advanced Array Methods

### Flatten and Compact

```ruby
# Flatten nested arrays
nested = [1, [2, 3], [4, [5, 6]]]
puts nested.flatten      # [1, 2, 3, 4, 5, 6]
puts nested.flatten(1)   # [1, 2, 3, 4, [5, 6]] (one level)

# Compact removes nil
arr = [1, nil, 2, nil, 3]
puts arr.compact         # [1, 2, 3]
```

### Zip and Transpose

```ruby
# Zip combines arrays
names = ["Alice", "Bob", "Charlie"]
ages = [30, 25, 35]
combined = names.zip(ages)  # [["Alice", 30], ["Bob", 25], ["Charlie", 35]]

# Transpose (for 2D arrays)
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
transposed = matrix.transpose  # [[1, 4, 7], [2, 5, 8], [3, 6, 9]]
```

### Group and Chunk

```ruby
# Group_by
words = ["apple", "banana", "cherry", "date", "elderberry"]
by_length = words.group_by(&:length)
# {5=>["apple"], 6=>["banana", "cherry"], 4=>["date"], 10=>["elderberry"]}

# Chunk
numbers = [1, 1, 2, 2, 2, 3, 1, 1]
chunks = numbers.chunk(&:itself).map { |n, arr| [n, arr.size] }
# [[1, 2], [2, 3], [3, 1], [1, 2]]
```

## Practical Examples

### Working with CSV-like Data

```ruby
# Parse CSV-like data
data = "name,age,city\nAlice,30,NYC\nBob,25,LA"
lines = data.split("\n")
headers = lines.first.split(",")
records = lines[1..-1].map do |line|
  values = line.split(",")
  headers.zip(values).to_h
end
# [{name: "Alice", age: "30", city: "NYC"}, {name: "Bob", age: "25", city: "LA"}]
```

### Array as Stack and Queue

```ruby
# Stack (LIFO)
stack = []
stack.push(1)     # [1]
stack.push(2)     # [1, 2]
stack.pop         # Returns 2, stack = [1]

# Queue (FIFO)
queue = []
queue.push(1)     # [1]
queue.push(2)     # [1, 2]
queue.shift       # Returns 1, queue = [2]
```

### Finding Duplicates

```ruby
arr = [1, 2, 3, 2, 4, 3, 5]
duplicates = arr.select { |e| arr.count(e) > 1 }.uniq
# [2, 3]

# More efficient way
duplicates = arr.group_by(&:itself).select { |k, v| v.size > 1 }.keys
# [2, 3]
```

## Performance Tips

1. Use `<<` instead of `push` for single elements (slightly faster)
2. Pre-allocate arrays when size is known: `Array.new(1000)`
3. Use `flat_map` instead of `map.flatten`
4. For large arrays, consider using `Set` for lookups
5. Use bang methods (!) when you don't need the original array

## Common Pitfalls

```ruby
# Modifying during iteration (dangerous!)
arr = [1, 2, 3, 4, 5]
# DON'T DO THIS:
# arr.each { |n| arr.delete(n) if n.even? }

# DO THIS instead:
arr.delete_if { |n| n.even? }
# or
arr = arr.reject { |n| n.even? }

# Array references
a = [1, 2, 3]
b = a          # b references same array
b << 4         # Both a and b are now [1, 2, 3, 4]

# To copy:
b = a.dup      # Shallow copy
b = a.clone    # Shallow copy with frozen state
```


