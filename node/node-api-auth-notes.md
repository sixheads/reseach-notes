# Build A Node.js API Authentication With JWT Tutorial -- Dev Ed

Source: https://www.youtube.com/watch?v=2jqok-WgelI&list=PLDyQo7g0_nsUIbQhYNVlM0u7kb-6ou4MQ

- first up we npm install express and nodemon
- nodemon is used for restarting our server

- we then setup our basic express server in an index.js file

```javascript
// required the express package
const express = require('express');
// set app equal to express which we invoke
const app = express();

app.listen(3000, () => console.log('Server up and running'));
```

## Routes

- we setup our routes in a separate folder
- our first register route is setup in auth.js

```javascript
const router = require('express').Router();

router.post('/register', (req, res) => {
  res.send('Register');
});

module.exports = router;
```

- we then import this into or index.js
- then setting a middleware we create the full route for anything in the register area

```javascript
// Import Routes
const authRoute = require('./routes/auth');

// Route Middleware
app.use('/api/user', authRoute);
```

## Database

- setup a new custer on MongoDB Atlas
- create a user & set the network access
- we then need to create a model in our app - basically shows how our data is going to look like
- to make working with MongoDB easier we use the **mongoose** package
- to connect to our DB we use mongoose connect:

```javascript
// Connect to DB
mongoose.connect(
  `mongodb+srv://sixheads:<password>@cluster0-kvsc1.mongodb.net/test?retryWrites=true&w=majority`,
  () => console.log('connected to db')
);
```

- we need to save our password details in a .env file so first we add the dotenv package : npm install dotenv

- next we create out model for our Users for interacting with out database - we describe the data with our model
