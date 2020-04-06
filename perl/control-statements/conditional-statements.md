When ever you want to perform a set of operations based on a condition(s) If / If-Else / Nested ifs are used.

You can also use if-else , nested IFs and IF-ELSE-IF ladder when multiple conditions are to be performed on a single variable.

# 1. If

## Syntax

```perl
if(conditional-expression)
{
    #code
}
```
## Example

```perl
  $x = 30;
  $y = 30;

  if ( $x == $y) {
    print "x and y are equal";
  }
```
### Check result [here](https://onecompiler.com/perl/3vnufqbpw)

# 2. If-else

## Syntax

```perl
if(conditional-expression)
{
    #code
} else {
    #code
}
```
## Example

```perl
$x = 30;
$y = 20;

if ( $x == $y) {
    print "x and y are equal";
} else {
    print "x and y are not equal";  
}
```
### Check result [here](https://onecompiler.com/perl/3vnufv3jb)

# 3. If-else-if ladder

## Syntax
```perl
if(conditional-expression-1)
{
    #code
} elsif(conditional-expression-2) {
    #code
} elsif(conditional-expression-3) {
    #code
}
....
else {
    #code
}
```

## Example
```perl
  $age = 79;

    if ( $age <= 1 && $age >= 0) {
      print "Infant";
    } elsif ($age > 1 && $age <= 3) {
        print "Toddler";
    } elsif ($age > 3 && $age <= 9) {
        print "Child";
    } elsif ($age > 9 && $age <= 18) {
        print "Teen";
    } elsif ($age > 18) {
        print "Adult";
    } else {
        print "Invalid $age";
    }
```
### Check result [here](https://onecompiler.com/perl/3vnufzdz5)

# 4. Nested-If

Nested-Ifs represents if block within another if block. 

## Syntax
```perl
if(conditional-expression-1) {    
     #code    
          if(conditional-expression-2) {  
             #code
             if(conditional-expression-3) {
                 #code
             }  
    }    
}
```

## Example
```perl
 $age = 50;
 $resident = 'Y';
 if ($age > 18)
  {
    if($resident == 'Y'){
      print "Eligible to Vote";
    }
  }
```
### Check result [here](https://onecompiler.com/perl/3vnugaqs2)

# 5. Unless

Unless is similar to If and is equivalent to `if-not`. Perl executes the code block if the condition evaluates to false,  otherwise it skips the code block.

## Syntax

```perl
statement unless(condition-expression);

#or

unless(condition-expression){
   #code 
}
```
## Example
```perl
$age = 35;
 
unless($age < 18){
   print("Major")                    
}
```
###  Check Result [here](https://onecompiler.com/perl/3vnuj6xme)

# 6. Unless-else

## syntax

```perl
unless(condition-expression){
  # unless code
}else{
  # else code
}
```

## Example

```perl
$x = 30;
$y = 30;

unless ( $x == $y) {
    print "x and y are not equal";
} else {
    print "x and y are equal";  
}
```
### Check result [here](https://onecompiler.com/perl/3vnujet2w)

# 7. Given

Given is similar to Switch in other programming languages. Given is an alternative to IF-ELSE-IF ladder and to select one among many blocks of code.

## Syntax
```perl
given(conditional-expression){    
when(value1){#code}
when(value2){#code}
when(value3){#code}
...
} 
```
