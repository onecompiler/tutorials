Express is one of the most popular web application framework in the NodeJS echosystem.

* pretty fast
* Minimalist
* unopinionated
* Very flexible

## How to install express

```sh
npm install express
```

## Hello World server example

```javascript
const express = require('express');
const app = express();

app.set('port', (process.env.PORT || config.port));

app.get('/', (req, res) => res.send('Hello World!'));

app.listen(app.get('port'), () => console.log(`Server started on ${app.get('port')} port`))
```

## Basic routing

```javascript
//GET
app.get('/', function (req, res) {
  res.send('Hello World!')
})

//POST 
app.post('/', function (req, res) {
  res.send('POST request. body:', req.body)
})

//DELETE
app.delete('/:id', function (req, res) {
  res.send('DELETE request for id:'. req.params.id)
})
```

## Static files serving

```javascript
app.use(express.static(__dirname + '/public'));
```

## logging all routes
```javascript
app._router.stack.forEach(function(r) {
    if (r.route && r.route.path) {
      console.log(r.route.path)
    }
  });
```

## defining routes in a different file

### `/routes/users.js` file

```javascript
// File Path: /routes/users.js

var express = require('express');
var router = express.Router();

router.get('/', (req, res) => {
    const users = []; // get from db
    res.send(users);
});

router.get('/:id', (req, res)=> {
    const user = {}; // get from db
    res.send(user);
});

router.post('/', (req, res) => {
    const user = req.body; // save user to db
    res.send({status: 'success'});
});

module.exports = router;
```

### adding routes from `/routes/users.js`

require() function is needed to include the modules.

```javascript
app.use('/user', require('./routes/user'));
```

## Redirects

```javascript
router.get('/old-path', function(req, res) {
  res.redirect('/temp-new-path'); // sends a 302
});

router.get('/old-path', function(req, res) {
  res.redirect(301, '/permanent-new-path'); // sends a 301
});
```