Perl supports Object oriented concepts. Perl object oriented concepts are based on references, anonymous arrays and hashes.

# 1. Classes and objects

Object is a basic unit in OOP, and is a single entity which combines data and actions. They have state and behaviour. Consider mobile as an object and let's see an example to understand state and behaviour. 

State of the objects can be represented by variables and behaviour of the objects can be represented by methods. Object is a real-world or run-time entity.

**Object** : Mobile
**State** : Model, Brand, Color, Size
**Behaviour** : Make a call, send a message, Click a picture

Class is a package which contains methods used to create and manipulate objects. In order to create a class, you need to build a package. 

### How to create a Class

`package` keyword is required to create a class.

```perl
package className; 
```
### How to create a Object

```perl
$object = new className( Attributes);
```
### How to define methods in a class

```perl
sub methodName{
  #code
}
```
## Example

```perl
package Mobile; # creating Class

sub new {
   my $class = shift;
   my $self = {
      _brandName => shift,
      _modelName  => shift,
      _price       => shift,
   };
   print "Brand name of the Mobile is $self->{_brandName}\n";
   print "Model name of the Mobile is $self->{_modelName}\n";
   print "Price is $self->{_price}\n";
   bless $self, $class;
   return $self;
}

$object = new Mobile( "iPhone", "iPhone 11 Pro", "999 dollars"); # creating Object
```

## Check Result [here](https://onecompiler.com/perl/3vp87uvsq)

# 2. Inheritance

Inheritance is a process of acquiring parent properties and behaviour by a child automatically. Hence you can either reuse or extend parent properties or behaviour in the child class. The main advantage of inheritance is code reusability. Perl has a special variable called `@ISA`. Consider we have a class named `Dogs` which inherits from `Animals` base class.

```perl
package Dogs;
use Animals;
use strict;
our @ISA = qw(Animals);    # inherits from Animals
```

# 3. Encapsulation

Encapsulation is a mechanism to protect private hidden data from other users. It wraps the data and methods as a single bundle. You can hide the complexity using objects in object oriented programming. Client of the objects will not know the internal logic and still can use the object using it's methods.

# 4. Polymorphism

Polymorphism gives the meaning many forms, usually it occurs when multiple classes are present and have been inherited.

Consider an example of installing an application in your mobile, where your base class is `News` and method is `subscribeNews()` and its derived classes could be Sports, Politics, Business, Weather etc which will have its own implementation but the method `subscribeNews()` can be common which can be inherited from its base class.
