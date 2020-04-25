Object destructuring is one of the powerful features introduced by ES6. Instead of writing multiple lines of code, we can simply assign the object to the variables as shown below.

```javascript
let user = { firstName: "foo", lastName: "bar", 
	     age: 27, junk: "bla"};

// Assigning to variables
let {firstName, age} = user;
let {firstName : f, age : a} = user;

console.log(`${f}'s age: ${a}`);
console.log(`${firstName}'s age: ${age}`);	// above and this line yields same result


// Using methods
function displayUserAge({firstName, age}){
	console.log(`${firstName}'s age: ${age}`);
}

displayUserAge(user);
```

## Run [here](https://onecompiler.com/javascript/3vr4bbqgs) 