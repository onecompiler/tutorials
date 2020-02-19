A function is a block of code which can be re-used multiple times. Beauty of functions is that they allow the code to call the functions multiple times and hence avoiding repetition of code.

## How to declare a function

### Syntax 
```javacript
function function-name(parameters){ // here parameters are optional
    //code
}
```

## How to call a function

### Syntax
```javascript
function-name(parameters);
```

## Examples

### 1. Function with out parameters

```javascript
function greetings(){
    console.log('Happy learning!!');
}
greetings();
greetings(); // You can call the function as many times you want
```
### check the result [here](https://onecompiler.com/javascript/3vhq6gvf4)

### 2. Function with parameters

```javascript
function greetings(name){
    console.log('Hi ' + name + '! Happy learning!!');
}
greetings('foo');
greetings('bar'); 
```
### check the result [here](https://onecompiler.com/javascript/3vhq64g82)

### 3.  Function with parameters having default values

If parameters are not provided while calling the function, then it's value becomes `undefined`.  If you want to default the parameter's values to some default text, then you can do this way.

#### Case : 1

If you are not providing any parameter then it's value becomes undefined.

```javascript
function setName(name){ 
	return name;
};
console.log(setName()); // undefined
console.log(setName("foo")); // foo
```
#### Case : 2

If you want to default a value to a parameter, here is how you can do it.

```javascript
function setName(name = "NA"){ // defaulting values to parameters
	return name;
};
console.log(setName()); // NA
console.log(setName("foo")); // foo
```
#### Case : 3

Below example helps you understand the difference between parameter value defaults and object literal defaults. 

```javascript
func = function({x = 10,y = 11,z = 12} = {x:1,y:2,z:3}) {
    console.log(x,y,z);
};

func(); //1 2 3  (hits the object literal default)
func({}); //10 11 12   (hits the value defaults)
```
### Check the result [here](https://onecompiler.com/javascript/3vhq7skve) 

## 4. Function Expressions

You can assign function to a variable explictly.  In Javascript, function is a value and hence you can assign it to a variable like any other values.

### Examples

```javascript
let message = function () {
    console.log('Happy Learning!');
}
```
You can copy function to an another variable.

```javascript
function greetings() {
    console.log('Happy Learning!');
}
let message = greetings; // if there are parameters to the function then you should specify greetings();

message(); // prints Happy Learning!
greetings(); // prints Happy Learning!
```

## Quick take away:

* A function can be assigned to a variable.
* You can default values to the parameters of a function
* A function is created predominently for re-usability.