In Javascript, class is similar to a function which you can think like kind of template which will get called when ever you initialize class. ES6 introduced classes along with OOPS concepts in JS.

## Syntax

```javascript
class className {
  constructor() { ... } //Mandatory Class method
  method1() { ... }
  method2() { ... }
  ...
}
```

## Example

```javascript
class Mobile {
  constructor(model) {
    this.name = model; 
  }
  features() {
    console.log("Handset Name:" + this.name);
  }  
}

console.log(typeof Mobile); //function

mbl = new Mobile("iPhone");
mbl.features();
```
### Run [here](https://onecompiler.com/javascript/3vr49fybj)

The above example creates a function named Mobile which is the result of the class declaration, whose code is taken from the constructor method and class methods will be stored in it's object.prototype.

