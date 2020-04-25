Loops are used to perform iterations on a block of code based on a criteria.

## 1. for()

for is used to iterate a set of statements based on a condition for fixed number of times.

### Syntax

```javascript
for(Initialization; Condition; Increment/decrement) {  
//code  
} 
```
### Example

```javascript
console.log('simple for loop');
for(let i=1;i<=10;i++) {
  console.log(i);
}
```
### Run [here](https://onecompiler.com/javascript/3vr496bu7)

## 2. for..in()

For..in loop is used to iterate over properties of an object.

### Syntax
```javascript
for(variable in Object) {  
//code  
}  
```
### Example

```javascript
let info = {
  name: "foo",
  id: 123
}

for (let x in info) {
  console.log(x); // prints keys of info which are name and id
}
```
### Run [here](https://onecompiler.com/javascript/3vr497r5m)

## 3. for..of()

For..of loop is used to iterate over values of an iterable object.

### Syntax
```javascript
for(variable of iterable-Object) {  
//code  
} 
```
### Example

```javascript
let mobiles = [ "iPhone", "Samsung", "OnePlus", "Pixel"];
for(let mbl of mobiles) {  
    console.log(mbl);
} 
```
### Run [here](https://onecompiler.com/javascript/3vr499gfb)

## 4. while()

while is also used to iterate a set of statements based on a condition. Usually while is preferred when number of iterations is not known in advance.

### Syntax

```javascript
while(condition) {  
//code 
}  
```

### Example

```javascript
 console.log('simple while loop');
let i=1;
while(i<=10) {
  console.log(i);
  i++;
}
```
### Run [here](https://onecompiler.com/javascript/3vr49amxk)

## 5. do-while()

Do-while is also used to iterate a set of statements based on a condition. It is mostly used when you need to execute the statements atleast once.

### Syntax
```javascript
do {  
//code 
} while(condition); 
```
### Example

```javascript
 console.log('simple do-while loop');
let i=1;
do {
  console.log(i);
  i++;
} while(i<=10);
```

### Run [here](https://onecompiler.com/javascript/3vr49c32n) 

