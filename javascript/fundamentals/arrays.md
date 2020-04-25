An array is a collection of items or values. Array Indexes starts with zero.

## Syntax

```javascript
let arrayName = [value1, value2,..etc];
// or
let arrayName = new Array("value1","value2",..etc);
```

## Example

```javascript
let mobiles=["iPhone", "Samsung", "Pixel"];

// accessing an array

console.log(mobiles[0]);

//changing an array element
mobiles[3] = "Nokia";
```

## Array Iteration Methods

ES6 has brought many improvements to Javascript language and made developer's job easy with the introduction of many new functions. Below are some of the most useful array methods in Javascript.

## 1. forEach()

forEach method is used to run a function on every element present in an array. This method can only be used on Arrays, Maps and Sets. 

You can either use callback functions or Arrow functions in forEach method.

Remember that you can not break, return or continue in forEach method like you do in traditional for loop. If you want to do so, then use exceptions in the call back function. 

### Syntax

```javascript
arrayname.forEach(function(err, doc) {
    //code
});
```

### Example

```javascript
const numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
const squaresOfEvenNumbers = [];

numbers.forEach(function(ele) {
  if(ele % 2 == 0)
    squaresOfEvenNumbers.push(ele*ele)
});

console.log(squaresOfEvenNumbers);
```

## 2. map()

This method is used to create a new array by running a function on every element of an array.

### Syntax

```javascript
arrayName.map(function(err, doc)) {
//code
});
// Arrow function can also be used in place of callback function
```

## 3. filter()

This method is used to create a new array by running a function on every element of an array based on a criteria .

### Syntax

```javascript
arrayName.filter(function(err, doc)) {
//code
});
// Arrow function can also be used in place of callback function
```

## 4. reduce()

This method is used to produce a single value by running a function on every element of an array.

### Syntax

```javascript
arrayName.reduce(function(err, doc)) {
//code
});
// Arrow function can also be used in place of callback function
```

### Example

```javascript
const numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

const squaresOfEvenNumbers = numbers
.filter(ele => ele % 2 == 0)
.map(ele => ele ** 2)
.reduce((acc, ele) => acc + ele);

console.log("sum of squares of even numbers:");
console.log(squaresOfEvenNumbers);
```
### Run [here](https://onecompiler.com/javascript/3vr486kyt)

## 5. find()

This method returns the first element of the array which passes the criteria.

### Syntax

```javascript
let find_value = arrayName.find(function(err, doc)) {
//code
});
// Arrow function can also be used in place of callback function
```

### Example

```javascript
let emp = [
 { name: "foo", age: 16 },
 { name: "bar", age: 13 },
 { name: "Mark", age: 25 },
 { name: "John", age: 35 }
 ];

firstMatch = emp.find(emp => emp.age > 18)
console.log(firstMatch);
```
### Run [here](https://onecompiler.com/javascript/3vr48b7kr)

## 6. indexOf()

This method returns the index of a particular element.

### Syntax 

```javascript
let index = arrayName.indexOf(element);
```

### Example

```javascript
let arr = ['foo', 'bar', 'mark'];

if(arr.indexOf('mark') > -1) {
  console.log('value is present in the array');
} else {
  console.log('value is not present in the array');
} 
```
### Run [here](https://onecompiler.com/javascript/3vr48cm5x)

Note: lastIndexOf() is similar to indexOf() but returns the index of last occurence of the element.

## 7. from()

This method creates a new instance of the array from an array-like or iterable object.

### Syntax 

```javascript
let newArray= Array.from(arrayLike[, mapFn[, thisArg]]);
```

### Example

```javascript
let numbers = [1, 2, 3, 4];

const squaresOfNumbers= Array.from(numbers, value => value * value);
console.log(squaresOfNumbers); 
```
### Run [here](https://onecompiler.com/javascript/3vr48dz9j)

## 8. every()

This method returns true if all the elements present in the array passes the criteria else false.

### Syntax

```javascript
Array.every(callback(element[, index[, array]])[, thisArg])
```

### Example

```javascript
let emp = [
 { name: "foo", age: 16 },
 { name: "bar", age: 13 },
 { name: "Mark", age: 25 },
 { name: "John", age: 35 }
 ];

adultCheck = emp.every(emp => emp.age > 18)
console.log(adultCheck);
```
### Run [here](https://onecompiler.com/javascript/3vr48fs37)

## 9. some()

This method returns true if atleast one element present in the array passes the criteria else false.

### Syntax

```javascript
Array.some(callback(element[, index[, array]])[, thisArg])
```

### Example

```javascript
let emp = [
 { name: "foo", age: 16 },
 { name: "bar", age: 13 },
 { name: "Mark", age: 25 },
 { name: "John", age: 35 }
 ];

adultPresent = emp.some(emp => emp.age > 18)
console.log(adultPresent);
```
### Run [here](https://onecompiler.com/javascript/3vr48gync)

## 10. includes()

This method returns true if an element is present in the array else false. You can also specify an optional second parameter like shown below which specifies the position to start the search.

### Syntax

```javascript
arrayName.includes(value-to-be-checked[, starting-search-index])
```

### Example

```javascript
let arr = ['foo', 'bar', 'mark'];

console.log(arr.includes('bar'));
console.log(arr.includes('bar',1)); // optional second parameter specifies the position to start the search.
```
### Run [here](https://onecompiler.com/javascript/3vr48j82d)
