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
## 2. while()

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

## 3. do-while()

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

### Practice your understanding [here](https://onecompiler.com/javascript) 
