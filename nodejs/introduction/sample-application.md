
## How to create a sample application

1. Create a folder with any name in your workspace, for example like helloworld
2. Go to command and navigate to the folder you have created.
3. Execute the below command to create the default package structure for you. Provide package name, description etc when prompted.

```
npm init
```
4. You can install the dependent modules by using below command.For example, we are installing express framework which will help you to develop applications with robust set of features.

```
npm i express`
```

5. Execute the below command to open the package.json in code.
```
code package.json
```

6. Now, create index.js and write your sample code like below.

```javascript
const express = require('express');
const server = express();
// above two lines is to use express framework

server.get('/', (req, res) => {
    res.send('Hello World!!');
});

server.listen(5000,()=>console.log('server started on 5000'));
```


## How to run the above code

1. Execute the below command in the terminal and ensure you are in the project directory.

```
node index.js
```

2. Go to browser, and hit http://localhost:5000/ path. You must see `Hello world!!` message in case of no errors.

