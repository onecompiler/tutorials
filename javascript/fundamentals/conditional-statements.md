## 1. IF

IF is used to execute a block of code based on a condition. 

### Syntax

```javascript
if(condition){
    // code
}
```
### Example

```javascript
let age = 20; 

if (age > 18) {
  console.log('Adult');
}
```
## 2. IF-ELSE

ELSE part is used to execute the block of code when the condition fails.

### Syntax
```javascript
if(condition){
    // code
} else {
    // code
}
```
### Example

```javascript

let age=7;

if(age<0) {
console.log('invalid age');
} else if (age<5 && age >0) {
console.log('Infant');
} else if (age>=5 && age<=18) {
  console.log('child');
} else if (age>18) {
  console.log("Adult");
}
```

### Practice your understanding [here](https://onecompiler.com/javascript) 

## 3. SWITCH

SWITCH is used to replace nested IF-ELSE statements.

### Syntax
```javascript
switch(condition){
    case 'value1' :
        //code
        [break;]
    case 'value2' :
        //code
        [break;]
    .......
    default :
        //code
        [break;]
}
```

### Example

```javascript
// what is hello in different languages
let language = "italian";

switch(language){
  case "french" :
    console.log('BONJOUR');
    break;
  
  case "spanish":
    console.log('Hola');
    break;
  
  case "italian" :
    console.log("Ciao");
    break;
  
  case "hindi" :
    console.log("Namaste");
    break;
  
  default:
    console.log('Hello');
}
```
### Practice your understanding [here](https://onecompiler.com/javascript) 
