Variables are like containers which holds the data values. A variable specifies the name of the memory location. 

Variables can be mutable or immutable in Scala. Mutable variables can be changed mutiple times even after assigning value to the variable but immutable variables can't be changed once assigned.

# Scope of variables

Depending on the place where the variables are been used, they can have three different scopes 

## 1. Fields

* Variables which belong to an object are referred as Fields.
* `var` or `val` is used to define these variables. 
* Field variables can be accessed through out the object. They can also be accessed outside depending on the access modifiers the field is declared with. 
* They can be both mutable and immutable in nature.

### Example

All the below three ways are valid.

```java
var id = 1
id = 1
var  id:Int = 1
```

## 2. Method Parameters

* These are the variables which are passed inside a method when it is called. 
* They are only accessible from inside the method but you can able to access the objects passed in if you have a reference to the object from outside the method.
* These are always immutable in nature. 
* `val` keyword is used to define method parameters.

## 3. Local Variables

* These are the variables which are declared inside a method and can be accessible only inside the method unless you return them from the method. 
* `var` or `val` is used to define these variables. 
* They can be both mutable and immutable in nature.

