
Variables are like containers which holds the data values. A variable specifies the name of the memory location. 
In Perl, there is no need to explicitly declare variables to reserve memory space. When you assign a value to a variable, declaration happens automatically.

## Naming convention

* Perl variables are case-sensitive
* Variables should consists of letters, numbers and underscore.
* Variables length can be up to 255 characters.
* Variable first letter must be a letter or underscore.

## Syntax

```perl
$var-name =value; #scalar-variable
@arr-name = (values); #Array-variables
%hashes = (key-value pairs); # Hash-variables 
```

## Example

```perl
$name = "OneCompiler"; #scalar-variable 
@arr-name = (values); #Array-variables
%hashes = (key-value pairs); # Hash-variables 
```
The above examples are valid variables, let us see some invalid variables in perl as below:

```perl
$1name = "1C" ; # invalid because variable starts with a number `1`
$first name = "foo"; # invalid because white space is present
$first-name = "foo"; # invalid because hypen(-) is present 
```

In some cases, there can be problems when you don't declare the variables explicitly. 

```perl
$fruit = "mango";
print "I love $fruits"
```
### Check result [here](https://onecompiler.com/perl/3vnmn3yty)

You might expect the output as `I love mango` but you might be surprised to see the output as `I love` without throwing any error. This is because `fruit` and `fruits` are two different variables and `fruits` is not assigned with any value. To avoid these king of mistakes while programming, Perl provides a pragma called `strict` which requires you to declare variable explicitly before using the variable is used. `my` is used to declare variables when you use `strict`.

The above example when you use `strict`

```perl
use strict;
my $fruit = "mango";
print "I love $fruits"
```
### Check result [here](https://onecompiler.com/perl/3vnmnd98y)

Run the above program to find the difference. 

`my` variable is lexically scoped which means variable is local to the enclosing block and can't be visible outside.

```perl
use warnings;
$where = 'outer';
print ("I'm " .$where. "\n");

if (where) {
  my $where = 'inner';
  print ("I'm " .$where. "\n");
}

print ("I'm " .$where. "\n");
```
### Check result [here](https://onecompiler.com/perl/3vnmnsvf3)