Sub-routine is a function which contains set of statements. Usually they are written when multiple calls are required to same set of statements which increases re-usuability and modularity.

Sub-routine allows you to divide your large lines of code into smaller ones. Usually the division happens logically such that each function performs a specific task and is up to developer. You can define a sub-routine anywhere in your program. You can also use sub-routines defined in some other programs. `use`, `do` or `require` statements are used to load sub-routines defined in other programs.

* `eval()` function is used to generate a sub-routine at run-time. 
* `sub` is the keyword used to define a sub-routine

## How to define a sub-routine

```perl
sub SubName  [PROTOTYPES] [ATTRIBUTES] {
  #code
}
```

## How to call a Function

```perl
&SubName;
#or
SubName(argument-list); # argument-list is optional
```

# Different ways of calling a sub-routine

You can call sub-routines in different ways as given below based on the criteria.

1. Sub-routine with no arguments and no return value.
2. Sub-routine with no arguments and a return value.
3. Sub-routine with arguments and no return value.
4. Sub-routine with arguments and a return value.

Let's look examples for the above types.

## 1. Sub-routine with no arguments and no return value.

```perl
greetings();

sub greetings()  
{  
    print "Hello world!!";  
}  
```
### check result [here](https://onecompiler.com/perl/3vnwvk5e7)

## 2. Sub-routine with no arguments and a return value.

```perl
$result = sum();
print "Sum : $result";

sub sum() {
   $x = 10;
   $y = 20;
 
   $sum = $x + $y;
   return $sum; # returning sum value
}
```
### check result [here](https://onecompiler.com/perl/3vnwvq4cd)

## 3. Sub-routine with arguments and no return value.

```c
#include <iostream>
using namespace std;

int sum(int , int );
int main()
{
  int x= 10, y = 20;
  sum(x,y); // passing arguments to function sum
}

int sum(int x , int y) {
   int sum;
   sum = x + y;
   cout << "Sum : " << sum;
   return 0;
}
```

### Check result [here](https://onecompiler.com/perl/3vnww9pqp)

## 4. Sub-routine with arguments and a return value.

```perl
sum(10,20);

sub sum() {
   $sum = 0;

   foreach $item (@_) {
      $sum += $item;
   }
print "sum is: $sum"
  
}
```
### Check result [here](https://onecompiler.com/perl/3vnww4ytp)
