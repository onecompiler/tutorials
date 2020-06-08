Variables are like containers which holds the data values. A variable specifies the name of the memory location. 

# Types of variables in Ruby

## 1. Global variables

Global variables will have global scope and are accessible any where in the program. They must begin with `$`.

### Example
```ruby
$var_scope = "GLOBAL"
```

## 2. Local variables

Local variables scope is confined to the block of it's initialization. They must start with a `lowercase letter` or `underscore (_)`. Once execution of the block completes, the local variables has no scope.

### Example
```ruby
_greeting = "Hello"
name = "foo"
age = 21
```

## 3. Instance variables

Instance variables belongs to one instance of a class and are accessible from any instance of the class within a method of the class. They starts with a `@` sign

### Example

```ruby
@name = "OneCompiler"
```

## 4. Class variables

Class variables belongs to the entire class and are accessible from anywhere inside the class. They have to be initialized before you use class variables. They must start with `@@`.

### Example

```ruby
@@id = 111
```
