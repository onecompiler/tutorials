A variable is a named storage for data to access it later in the program. There are three ways to create variables.

|Keyword|Description|Scope|
|----|----|----|
|var| Var is used to declare variables(old way of declaring variables)| Function or global scope| 
|let| let is also used to declare variables(new way)|Global or block Scope|
|const|const is used to declare const values. Once the value is assigned it can not be modified|Global or block Scope|

## Example program to understand the difference between var, let and const

Check the below program to understand issues with var. In the below example, age is a `var` variable whose value is not getting reflected back to 20 even after coming out of if block but with let, name will again becomes `foo`. Hence, `let` is recommended to use.

```javascript

let name ="foo"; 
console.log('name:', name); //global scope, here name = foo
const id =1;
console.log('id:', id); //global scope, here id=1
var age = 20;
console.log('age:', age); //global scope, here age = 20

if(id) { 
    let name ="bar"; //block scope
    console.log('inside block name:', name); //here name = bar
    //const id =1;
    // if you uncomment the above line, program will throw an error as const can't be assigned twice
    console.log('id:', id); //id =1 
    var age = 25;
    console.log('age:', age); // here age=25
}
console.log('outside block name:', name); //here name will be foo again
console.log('id:', id); //id =1
console.log('age:', age); // age will not become 20 and it remains 25 even after the block is ended         
```
### Try yourself [here](https://onecompiler.com/javascript/3vr47v9hd)

## How to declare variables

```javascript
let variable-name; // Just declaration
let variable-name = value; // declaring variable and assigning it with some value
let var1 = value1, var2 = value2, var3 = value3; // multiple variables declaration with their assigned values
```
You can change the value of a variable multiple times with different data, Javascript will not throw any error if you assign string value to a integer variable.

```javascript
let x = 10;
x= "hello"; // don't throw any error and x will assigned to "hello"
```

### Practice your understanding [here](https://onecompiler.com/javascript) 
