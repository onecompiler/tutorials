Methods are similar to functions in other programming languages which contains a set of statements. Usually methods are written when multiple calls are required to same set of statements which increases re-usuability and modularity.

## How to define a method

Method name are always starts with a lowercase letter.

```ruby
def methodName [( [arg [= default]]...[, * arg [, &expr ]])]
   #code  
end 
```
### Example : 1

```ruby
def greetings(name = "Foo") # defaulting name as foo 
   puts "Hello #{name}"
end
greetings "John"
greetings
```
### Try yourself [here](https://onecompiler.com/vb/3vu555dhn)

### Example : 2

```ruby
# Sum of two intergers in ruby
def sum(x , y)
   sum = x+y
   puts "Sum: #{sum}"
end

sum(10,20)
```
### Try yourself [here](https://onecompiler.com/ruby/3vv74pvxd)

