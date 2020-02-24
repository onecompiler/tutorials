Modules are nothing but the scripts splitted over multiple files. Modules are essential to increase the readability of your code especially incase of larger projects. 

In the initial days, scripts were smaller and there was no need for modularity. But as the Javascript capability grows, we are using Javascript to build much more large and complex projects where modularity is required to manage the applications.

Defining modules is completely based on developer's choice. There is no strict guideline to it howeverkeeping a particular category under a module makes sense.

For example, you can keep api, config, router, services seperately.

## How to create Modules

* Modules interchange their functionality, call functions with one another using `export` and `import` directives.
* Create folder under your project and create the JS files as required.
* You need to initialize the dependencies if you are using them in the module javascript file.
* Finally export them at the end of your code in the module javascript file.
* For example, consider you have created the /routes/users.js file and exported as `module.exports=router;`

## How to use them in your main file

* Add similar to the below line to access them

```javascript
app.use('/user', require('./routes/user'));
```

## Example

Below example shows you to how to export functions from one js file to another.

```javascript
//sum.js
export function sum(a, b) {
  console.log(a+b);
}
```
```javascript
//index.js
import {sum} from './sum.js';

console.log(sum); // function...
sum(7, 6); // prints 13
```
### Practice your understanding [here](https://onecompiler.com/javascript) 
