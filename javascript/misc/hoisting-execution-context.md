
## Hoisting

Hoisting is a very popular concept in JavaScript. Lets understand it in a simple way. So, Hoisting is a concept in JavaScript in which all the Variable declarations, function declarations, class declarations are moved on top of the scope. Thus, we can safely use a variable before its declaration. Also, we can call the functions safely before their declarations. so first we will see how the functions are hoisted :

### Function Hoisting

```
printAge(18);  // output : 18
function printAge(age){
      console.log(age);
}

```
In the above code, due to hoisting, we can call the function printAge() before its declaration. This is an example of Function hoisting.

### Variable Hoisting

In JavaScript, all the variables are hoisted. In case of let and const variables we get reference error because of temporal dead zone. In case of var variable we get undefined because a var variable is always initialized with undefined when it is declared. Note that, a Variable is hoisted only if it is declared or it is declared and initialized at the same time. The variables which are only initialized without declaring are not hoisted. let us understand this through example:

### declaration and initialization

```
 var num; //declaration
 num = 12;  //initialization
 var num = 12 // Declaring and Initializing at the same time

```

```
//variable Hoisting
console.log(a);   // output : undefined
var a = 12; // initialization
console.log(a); // output : 12

```
```
console.log(b); // output : referenceError
let b = 13; 

// Same behaviour in case of const variable also

```
If we only initialize a variable without declaring, then that variable is not hoisted. for example below code :
```
console.log(a); // output : referenceError
a = 25;

```
### Advantage of Hoisting

Advantage: The main Advantage of Hoisting is We don't have to see where the variables and functions are declared and what is there scope. because due to hoisting, all the declarations are moved to the top of the scope.

## Temporal Dead Zone

Whenever we run any JavaScript Program A Global Execution Context is created and in its memory component, All the Variables declared with "var" are assigned a special keyword called "undefined". So, if we try to access that variable before its declaration, it will output "undefined" (due to Hoisting) which is totally fine. But, In case of "let" and "const" variables, the Scenario changes. These variables do not get assigned with any value or keyword. Therefore, When we try to access them before there declaration, It will give us a "ReferenceError". Thus, as we are unable to access these "let" and "const" variables before there declaration, these variables are said to be in Temporal Dead Zone till they are declared and initialized. After that, they are out of the Temporal Dead Zone. let us understand this through some examples. Note that Temporal Dead Zone starts from beginning of the scope till the declaration and initialization of let and const variables.

```
  console.log(a);  //undefined
  console.log(b);  //referenceError: cannot access b before initialization
  console.log(b);  //referenceError: cannot access c before initialization

  var a = 15;
  let  b = 25;
  const c = 35;

```
In the above code snippet, As variable a is declared and initialized using "var" it got assigned with "undefined" and the variables b and c are declared and initialized using "let" and "const" therefore, due to temporal dead zone we cannot access them before their declaration and initialization.

```
function getValue(){
console.log(a);
}
getValue(); // ReferenceError: Cannot access 'a' before initialization
let a = 15;  // temporal dead zone for variable a ends here
getValue();  // output : 15

```
In the above code, initially the function getValue is declared and after that it is called once before the declaration and initialization of variable a and again called once after the declaration and initialization of variable a. Now, as we know the let and const variables are in Temporal Dead Zone till they initialized. So, when getValue() is called before initialization of a, due to temporal dead zone it will output referenceError. On the other hand, when the getValue() is called after the initialization of a i.e. out of the Temporal Dead Zone, the output will be the value assigned to a i.e. 15.

## Execution Context 

Whenever a JavaScript engine (V8) receives a JavaScript file or a Script file, A Global Execution Context is Created. Now, this Global Execution Context contains two parts one is Memory part and another is Code part. Here is how a Global Execution context looks like :

| MEMORY | CODE |
| ---- | ---- |
| ---- | ---- |
| ---- | ---- |
| ---- | ---- |

Global Execution Context

The Memory part of Global Execution Context contains all the variables, functions in the javascript file and allocate memory for all of them in the form of Key-Value Pairs. The Code component contains the lines of code which get executed one-by-one.

## Call Stack

In a big Javascript program, there are many execution contexts created for various functions and code blocks. Now, the question arises, "How the JS Engine manages so many Execution contexts?" So, the answer is it uses Stack and pushes newly created context in the stack known as Call Stack. The Call Stack maintains the order of creation and execution of the execution contexts.

