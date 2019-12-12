An array is a collection of items or values. 

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

## Array Methods

   * `pop()` method is to remove the last element present in the array
   * `push()` method is used to add last element at the end.
   * `shift()` is used to remove first elememt and shifts other elements by one lower index.
   * `unshift()` is used to add an element to the first position and shifts the rest sideways.
   * `tostring()` us used to convert array to a string.
   * `splice()` is to add / remove / modify any element present in an array.

## Array Iteration Methods

## 1. forEach()

This method is used to run a function for every element present in an array.

### Syntax

```javascript

arrayname.forEach(function(err,doc){
    //code
});
// Arrow function can also be used in place of callback function
```
### Practice your understanding [here](https://onecompiler.com/javascript) 

## 2. map()

This method is used to create a new array by running a function on every element of an array.

### Syntax
```javascript
arrayName.map(function(err,doc)){
//code
});
// Arrow function can also be used in place of callback function
```

## 3. filter()
This method is used to create a new array by running a function on every element of an array based on a criteria .

### Syntax
```javascript
arrayName.filter(function(err,doc)){
//code
});
// Arrow function can also be used in place of callback function
```


## 4. reduce()
This method is used to produce a single value by running a function on every element of an array.

### Syntax
```javascript
arrayName.reduce(function(err,doc)){
//code
});
// Arrow function can also be used in place of callback function
```
### Note
You can also concatenate the above methods based on the requirement. Check the below example for better understanding.

### Example
```javascript
const numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

const squaresOfEvenNumbers = numbers
.filter(ele => ele % 2 == 0)
.map(ele => ele ** 2)
.reduce((acc, ele) => acc + ele);

console.log(squaresOfEvenNumbers);
```

## 5. find()

This method returns the first element of the array which passes the criteria.

### Syntax
```javascript
let find_value = arrayName.find(function(err,doc)){
//code
});
// Arrow function can also be used in place of callback function
```
Note: findIndex() is similar to find() but returns the index of the element instead of its value.

## 6. indexOf()

This method returns the index of a particular element.

### Syntax 
```javascript
let index = arrayName.indexOf(element);
```

Note: lastIndexOf() is similar to indexOf() but returns the index of last occurence of the element.

