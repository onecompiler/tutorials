Groovy closure is an open, anonymous, block of code. Closure can take arguments and can return a value. They can be assigned to a variable as well.

## Syntax

```java
{ [closureParameters -> ] statements }
```

* [closureParameters ] is the 0 or more parameters you can provide and should be seperated by `,`. They are optional.
* `->` is used to seperate closure parameters from it's body
* statements can be 0 or more statements where you write the closure body.

Below are some of the examples of closure definitions

```java
{ x++ }         // only statement                                 

{ -> x++ }     // statement with no parameters                                  

{ name -> println name }  // statement with one closure parameter  with no type definition                         

{ String name, int id ->   // statement with two closure parameter has type definition                          
    println "hi ${name}, your id is ${id}" 
}
```
## Parameters

Closure parameters can be implicit or explicit.  When a closure does not explicitly define a parameter, then closure always defines an implicit parameter called `it`. 

### Implicit

```java
def msg = { "Hello, $it!" }
assert msg('world') == 'Hello, world!'
```
the above code is equivalent to the below one:

```java
def msg = { it -> "Hello, $it!" }
assert msg('world') == 'Hello, world!'
```


### Explicit

```java
// explicit with no typedef
def sum = { a,b -> a+b }
assert sum(10,20) == 30

// explicit with no typedef
def sumWithExplicitTypes = {int a, int b -> a+b }
assert sumWithExplicitTypes(10,20) == 30
```

Lists, Maps, and String methods accepts closure as an argument. 

## Using Closures with Lists

```java
def list1 = [1, 2, 3, 4, 5];
list1.each {println it}
```
### Try yourself [here](https://onecompiler.com/groovy/3vmy99wb9)

## Using Closures with Maps

```java
def map1 = ["Name" : "OneCompiler", "Category" : "Learning"]             
map1.each {println it}
map1.each {println "${it.key}'s value is: ${it.value}"}
```
### Try yourself [here](https://onecompiler.com/groovy/3vmyjsdpc)


## Closure methods

| Method name | Description |
|----|----|
| any() & every() | Iterates through each element of a collection checking whether given criteria is valid for at least one element.|
| find() | Finds the first value in a collection which matches the given criteria. |
| findAll() | Finds all values in a collection which matches the given criteria.|
| collect() | Iterates through a collection and converts each element into a new value using the closure as the transformer.|

### Example

```java
def list1 = [1,2,3,4,5,6,7,8,9,10];
def result;
		
println "First even number is :" + list1.find{ele -> ele % 2 == 0} // find method to get the first value which satisfies the criteria

println "List of all even numbers is: "
result = list1.findAll{ele -> ele % 2 == 0} // findall method to get all even numbers.
result.each {println it}
```
### Try yourself [here](https://onecompiler.com/groovy/3vmyk9vzm)

 