ES6 introduced these two powerful keywords called let and const.


|Keyword|Description|Scope|
|----|----|----|
|var| Var is used to declare variables(old way of declaring variables)| Function or global scope| 
|let| let is also used to declare variables(new way)|Global or block Scope|
|const|const is used to declare const values. Once the value is assigned it can not be modified|Global or block Scope|


Let's get familiarize with the below scopes.

### Global scope

Variables declared globally will have global scope

### Function scope

Variables declared inside a function will have scope only in that function.

### block scope

Variables declared in a block like inside a loop will have block scope.

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

## Practice your understanding [here](https://onecompiler.com/javascript) 
