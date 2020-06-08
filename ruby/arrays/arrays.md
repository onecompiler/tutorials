Array is a collection of data items which is stored in continuous memory addresses. Array values can be fetched using index. 

Index starts from 0 to size-1.

Unlike in other traditional programming languages, Ruby arrays can hold objects of different data types such as String, Integer, Fixnum, Hash, Symbol etc.

## How to create an array?

You can create array using either `new` keyword or using `[]` literal.
```ruby
array_name = Array.new(size)
# or
array_name = Array.new
```
### Example

```ruby
arr = Array.new(10) # creates an array of 10 data items
arr = [ "Good", "morning", 9, 5.32 , true] # creates an array using []
```

## How to access array elements

Array elements can be accessed by using indices. Array indices starts from `0`.  `Array(n-1)` can be used to access nth element of an array.

## Example: 1

```ruby
directions = ["East", "West", "North", "South"]
puts("#{directions}") # prints the elements present in an Array

puts directions.at(3) # prints the element at 3rd index

puts directions.at(-1) # prints the element at -1 index which is first element from last
```
### Try yourself [here](https://onecompiler.com/ruby/3vv6xdur7)

## Example: 2

```ruby
directions = ["East", "West", "North", "South"]

$i = 0;
$n = directions.length

puts("\nPrinting array elements using while loop:\n")
while $i <= $n do 
  puts(directions.at($i))
  $i += 1
end
```
### Try yourself [here](https://onecompiler.com/ruby/3vv6y7axj)


