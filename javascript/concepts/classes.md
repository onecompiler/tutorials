ES6 introduced classes along with OOPS concepts in JS. Class is similar to a function which you can think like kind of template which will get called when ever you initialize class.

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
}

mbl = new Mobile("iPhone");
```
