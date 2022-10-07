
# Promises

* To handle asynchronus operations
* Efficient way instead of multiple complex call back functions which makes code very large and unmanagable.
* Makes code look simple and easy to understand.

## Promises states

|States|Description|
|----|----|
| Rejected | If the action specified in the promise is failed|
| Fulfilled | If the action specified in the promise is fulfilled|
| Settled | Either it is fulfilled or rejected|
| Pending | If the action specified in the promise is still processing|

## Syntax

```javascript
let promise = new Promise(function(resolve, reject){
    //code
}); 
```
### Note
* If the action specified in the code of Promise is fulfilled, then the result will be present in resolve(value) which is again a call back function.
* If the action specified in the code of Promise is rejected, then the error details will be in reject(error)

## Example

```javascript
let promise = new Promise(function(resolve, reject) { 
let x = 3; 
let y = 9;
let val= x*y;

if(val%2 == 0) { 
	resolve(); 
} else { 
	reject(); 
} 
}); 

promise. 
	then(function () { 
		console.log('Result is an even number'); 
	}). 
	catch(function () { 
		console.log('Rejected'); 
	}); 
```
### Run [here](https://onecompiler.com/javascript/3vr4b4h7g)

# Async-Await

* Async-Await are the more cleaner and efficient way of writing asynchronous operations in JS. 
* Efficient way instead of multiple complex promises which makes code very large and unmanagable.
* Makes code look simple and easy to understand.
* Async functions always return a value, either returns value if the promise is fulfilled or error if the promise is rejected.

## Syntax
```javascript
async function functioname(parameters){
	//code
}
```

## Example

```javascript
async function getTodos(userObj){
	const res = await fetch([url]);
	const data = await res.json()    
	return data;
}

let data = await getTodos({fn: "foo"});
```
## Fetching API data using both promise method and async and await method.

- promise

```js
const url = 'https://restcountries.com/v2/all'
fetch(url)
  .then(response => response.json())
  .then(data => {
    console.log(data)
  })
  .catch(error => console.error(error))
```

- async and await

```js
const fetchData = async () => {
  try {
    const response = await fetch(url)
    const countries = await response.json()
    console.log(countries)
  } catch (err) {
    console.error(err)
  }
}
console.log('===== async and await')
fetchData()
```
