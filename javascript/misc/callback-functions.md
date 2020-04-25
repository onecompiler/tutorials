Javascript is an event-driven and asynchronous programming language means we can initiate any action now and finish them later without disturbing the flow of the program.

In Javascript, almost everything is treated as objects. Functions are no exception to it. Hence functions can even take another function as it's argument and can be returned by other functions. In simpler terms, any function that is passed as an argument is called a callback function. JavaScript Callback Functions can be used synchronously or asynchronously.

## Example of asynchronous execution

```javascript

function a() {
    //DB call or api call
}

function b() {
    // plain code which doesn't take much time
}

a();
b();
```
Consider you are making some DB calls or api calls in a() function and less execution time code in b() function. Though you call a() function first,  b() function may be in results first and then followed by a(). Because javascript will not wait for a() function to complete it's execution before proceeding to b() function. Callbacks are a solution to such kind of scenarios, where certain code doesn’t execute until other code has already finished execution. 

## Example for Callback function

```javascript
function a(x) { return x; };

function b(parameter1) {
    // code
}

b(a);
```

You can also pass as many callback functions you like to another function.

```javascript
function a(x) {
    return x; 
}

function b(n1, n2, callback1, callback2) {
    // code
}

b(1, 2, a, a);
```
