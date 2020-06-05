## 1. For

For loop is used to iterate a set of statements based on a condition. Usually for loop is preferred when number of iterations are known in advance.

### Syntax

```ruby
for variable [, variable ...] in range [do]
   #code
end
```
### Example

```ruby
for i in 1..10
   puts "i: #{i}"
end	
```

### Try yourself [here](https://onecompiler.com/ruby/3vv46pt52)


## 2. While

While is also used to iterate a set of statements based on a condition. Usually while is preferred when number of iterations is not known in advance.

### Syntax

```ruby
while conditional-expression [do]
   #code
end
```
### Example

```ruby
$i = 1
$n = 10

while $i <= $n  do
   puts("i = #$i" )
   $i +=1
end
```
### Try yourself [here](https://onecompiler.com/ruby/3vv46vb6n)

## 3. Until

Until is also used to iterate a set of statements based on a condition. 

### Syntax

```ruby
until conditional-expression [do]
   #code
end
```
### Example

```ruby
$i = 1
$n = 10

until $i > $n  do
   puts("i = #$i" )
   $i +=1;
end
```

### Try yourself [here](https://onecompiler.com/ruby/3vv4736nt)
