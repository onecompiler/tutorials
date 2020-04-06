# 1. For

For loop is used to iterate a set of statements based on a condition. Usually for loop is preferred when number of iterations are known in advance. In perl, `for` and `foreach` loops are interchangeable, and hence you can use foreach loop in where you use the for loop.

## Syntax

```perl
for(Initialization; Condition; Increment/decrement){  
#code  
} 

#or

for (range){
  #code
}
```
## Example

```perl
@x = (1..5);

print "Using For loop:\n";
for(@x){
 print("$_","\n");
}


print "\nUsing For-each loop:\n";
foreach(@x){
 print("$_","\n");
}
```
Both for and for-each loops produce same result.

### Check Result [here](https://onecompiler.com/perl/3vnujqurj)

Below example shows how to loop through hashes.

```perl
%nationalGame = (Australia=>'Cricket',
                 Japan => 'Wrestling',
                 NewZealand => 'Rugby',
                 USA => 'Baseball',
                 England => 'Cricket');

# Loop through hash elements
for(keys %nationalGame){
 print("National Game of $_ is $nationalGame{$_}\n");
}
```
### Check result [here](https://onecompiler.com/perl/3vnwnghgy)

# 2. While

While is also used to iterate a set of statements based on a condition. Usually while is preferred when number of iterations is not known in advance.

## Syntax

```perl
while(condition){  
#code 
}  
```
## Example

```perl
$i=1;
print "Using while loop:\n";
while ( $i <= 5) {
 print "$i\n" ;
  $i++;
}
```
### Check result [here](https://onecompiler.com/perl/3vnwnmp75)

# 3. Do-while

Do-while is also used to iterate a set of statements based on a condition. It is mostly used when you need to execute the statements atleast once.

## Syntax

```perl
do{  
#code 
} while(condition); 
```
## Example

```perl
$i=1;
print "Using do-while loop\n";
do {
  print"$i\n";
  $i++;
} while ($i<=5);

```

### Check result [here](https://onecompiler.com/perl/3vnwntcpj)

# 4. Until

Until is just opposite of while loop and executes a block of code as long as the condition is false.

## Syntax

```perl
until(conditional-expression){
   # code
}
```
## Example

```perl
$i=1;
print "Using until loop:\n";
until ( $i > 5) {
 print "$i\n" ;
  $i++;
}
```
### Check result [here](https://onecompiler.com/perl/3vnwpabyg)

# 5. Do-Until

Do-Until is also used to iterate a set of statements based on a condition. It is mostly used when you need to execute the statements atleast once and it executes a block of code as long as the condition is false.

## Syntax
```perl
do{
   # code
}until(condition-expression)
```

## Example
```perl
$i=1;
print "Using do-until loop\n";
do {
  print"$i\n";
  $i++;
} until ($i > 5);
```
### Check result [here](https://onecompiler.com/perl/3vnwpkm3k)
