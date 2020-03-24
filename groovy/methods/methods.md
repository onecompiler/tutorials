Method is a sub-routine which contains set of statements which increases re-usuability and modularity.

## How to define a Method

```java
def methodName(parameters) { 
   //Method code 
}
```
## Examples

```java
// simple example to show a methos without parameters
greetings();

def greetings()
{
  println("Hello World!!")
}
```
### Check result [here](https://onecompiler.com/groovy/3vmytbapg)

```java
// Example for method with parameters
sum(10,20)

def sum( int x, int y){
  def result = x + y;
  println "Sum of $x and $y is $result"
}
```
### Check result [here](https://onecompiler.com/groovy/3vmyt4pha)

You can also default parameters to some values.

```java
greetings("foo");
greetings();
def greetings(String name = "NA")
{
  println("Hello $name!!")
}
```
### Check result [here](https://onecompiler.com/groovy/3vmythf4c)


