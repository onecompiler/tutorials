When ever you want to perform a set of operations based on a condition if and if-else are used.

## 1. If

```ruby
if conditional-expression 
    #code
end
```
### Example

```ruby
x = 10
y = 20

if (x != y) 
  print (" x and y are not equal")
end

```
### Try yourself [here](https://onecompiler.com/ruby/3vv44wfuz)

## 2. If-else

```ruby
if conditional-expression
    #code
else 
    #code
end
```
### Example

```ruby
x = 10
y = 10

if (x != y)  
  print (" x and y are not equal")
else 
  print ("x and y are equal")
end
```
### Try yourself [here](https://onecompiler.com/ruby/3vv452mn5)

## 3. Nested If-else

```ruby
if conditional-expression
    #code
elsif conditional-expression
    #code
else 
    #code
end
```

### Example

```ruby
x = 10
y = 20

if (x > y) 
  print (" x is greater than y")
elsif (x < y ) 
  print ("x is less than y") 
else 
  print ("x and y are equal")
end
```
### Try yourself [here](https://onecompiler.com/ruby/3vv4554xf)


## 4. Case

Case is similar to Switch statement in other programming languages.

```ruby
case exp
[when exp [then]
   #code ]...
[else
   #code ]
end
```
### Example

```ruby
$grade =  78
case $grade
when 1 .. 35
   puts "Fail"
when 36 .. 70
   puts "Pass"
when 71 .. 100
   puts "Pass with distinction"
else
   puts "Invalid grade"
end
```
### Try yourself [here](https://onecompiler.com/ruby/3vv45fqcy)