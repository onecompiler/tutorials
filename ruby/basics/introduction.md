# Introduction to Ruby

Ruby is a dynamic, open-source programming language with a focus on simplicity and productivity. Created by Yukihiro Matsumoto ("Matz") in 1995, Ruby has an elegant syntax that is natural to read and easy to write.

## Philosophy

Ruby follows the principle of "least surprise" - the language should behave in a way that minimizes confusion for experienced users. Matz designed Ruby to make programmers happy and productive.

## Key Features

* **Everything is an Object**: In Ruby, everything is an object, including primitive data types
* **Dynamic Typing**: Variables don't have types, values do
* **Duck Typing**: If it walks like a duck and quacks like a duck, it's a duck
* **Blocks and Iterators**: Powerful constructs for functional programming
* **Metaprogramming**: Code that writes code
* **Garbage Collection**: Automatic memory management
* **Rich Standard Library**: Extensive built-in functionality
* **Cross-platform**: Runs on Windows, macOS, Linux, and more
* **Great for Web Development**: Powers Ruby on Rails framework
* **Expressive Syntax**: Reads almost like natural language
* **Open Source**: Free to use, modify, and distribute

## Installation

### macOS

Ruby comes pre-installed on macOS, but you can install a newer version:

```bash
# Using Homebrew
brew install ruby

# Using rbenv (recommended for managing multiple versions)
brew install rbenv ruby-build
rbenv install 3.2.0
rbenv global 3.2.0
```

### Windows

1. Download RubyInstaller from [rubyinstaller.org](https://rubyinstaller.org/)
2. Run the installer and follow the setup wizard
3. Make sure to check "Add Ruby executables to PATH"
4. Install MSYS2 development toolchain when prompted

### Linux

```bash
# Ubuntu/Debian
sudo apt-get update
sudo apt-get install ruby-full

# CentOS/RHEL/Fedora
sudo yum install ruby

# Using rbenv (recommended)
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
source ~/.bashrc
```

### Verify Installation

```bash
ruby -v
# Output: ruby 3.2.0 (or your version)

irb
# Opens Interactive Ruby console
```

## Using OneCompiler

For quick testing without installation:
1. Visit [OneCompiler Ruby](https://onecompiler.com/ruby)
2. Write your code and run instantly
3. Perfect for learning and experimentation

## Your First Ruby Program

```ruby
# hello.rb
puts "Hello, World!"

# Run with: ruby hello.rb
```

## Basic Syntax Elements

```ruby
# Single line comment

=begin
Multi-line
comment
=end

# Variables (no declaration needed)
name = "Ruby"
version = 3.2
is_awesome = true

# String interpolation
puts "Welcome to #{name} version #{version}"

# Methods
def greet(name)
  "Hello, #{name}!"
end

puts greet("World")

# Classes
class Person
  def initialize(name)
    @name = name  # Instance variable
  end
  
  def say_hello
    puts "Hi, I'm #{@name}"
  end
end

person = Person.new("Alice")
person.say_hello
```

## Ruby vs Other Languages

| Feature | Ruby | Python | Java |
|---------|------|--------|------|
| Typing | Dynamic | Dynamic | Static |
| Syntax | Very flexible | Clean, strict | Verbose |
| OOP | Everything is object | Multi-paradigm | Class-based |
| Performance | Moderate | Moderate | Fast |
| Use Case | Web, Scripts | AI/ML, Web | Enterprise |

## Common Use Cases

1. **Web Development**: Ruby on Rails, Sinatra
2. **DevOps**: Chef, Puppet, Vagrant
3. **Testing**: RSpec, Cucumber
4. **Static Sites**: Jekyll, Middleman
5. **Game Development**: Gosu, Ruby2D
6. **Data Processing**: Scripts and automation

## Next Steps

Now that you have Ruby installed and understand the basics, you're ready to explore:
- Data types and variables
- Control structures
- Object-oriented programming
- The Ruby ecosystem and gems

