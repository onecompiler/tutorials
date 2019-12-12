Modules are nothing but the scripts splitted over multiple files. Modules are essential to increase the readability of your code especially incase of larger projects. 

Defining modules is completely based on developer's choice. There is no strict guideline to it howeverkeeping a particular category under a module makes sense

For example, you can keep api, config, router, services seperately.

## How to create Modules

* Create folder under your project and create the JS files as required.
* You need to initialize the dependencies if you are using them in the module javascript file.
* Finally export them at the end of your code in the module javascript file.
* For example, consider you have created the /routes/users.js file and exported as `module.exports=router;`

## How to use them in your main file

* Add similar to the below line to access them

```javascript
app.use('/user', require('./routes/user'));
```

### Practice your understanding [here](https://onecompiler.com/javascript) 
