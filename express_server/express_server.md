## How to Set Up an Express Server

1. Navigate to your project directory.
2. Touch server.js
3. Run npm init -y
4. Run npm i express (may also need mongoose, dotenv, ejs, etc.)
  - AKA install all your dependencies!
5. Touch .env file (for your private variables)
6. Inside of your .env, assign your PORT:
  - PORT=3000 (or whatever number close to 3000 you choose)
7. Code your DEPENDENCIES, CONFIGURATION, and LISTENER into your server.js file
8. Create any necessary controller files - aka, specific routers for your app. (For example, for a restaurant app, you might have separate controllers for food, and drinks.)
9. INSIDE YOUR CONTROLLER FILES:
  a. require express: const express = require('express')
  b. Create a variable for your express router: const router = express.Router()
  c. Set up a basic route:
    router.get('/', (req, res) => {
      res.send("This test message should appear when you visit localhost:PORTvar")
      })
  d. Make sure to export your controller's router: module.exports = router
10. If connecting to mongoose and the back end (which you probably are), you'll need to set up a Mongo DB connection string in your .env file as well, for example:
  - MONGODB_URI=mongodb+srv://cheisentrout:l0st03l>>@unit2project.dwhpd.mongodb.net/birds?retryWrites=true&w=majority
11. Configure MongoDB in your server.js file as such:
  - const MONGODB_URI = process.env.MONGODB_URI (so that your express server has access to your MongoDB database)
12. Configure Mongoose, add mongoose error messages
13. Add any necessary MIDDLEWARE
14. Should look something like the code below!


  **********************************************
  /*============ DEPENDENCIES =============*/

  const express = require('express')
  const mongoose = require('mongoose')

  /*============ CONFIGURATION =============*/

  const app = express()
  require('dotenv').config()
  const PORT = process.env.PORT
  const MONGODB_URI = process.env.MONGODB_URI

  mongoose.connect(MONGODB_URI, {
    useNewUrlParser: true,
    useUnifiedTopology: true,
    useFindAndModify: false
  })

  /*============ MONGOOSE ERR/SUCCESS =============*/

  mongoose.connection.on('error', err =>
    console.log(
      err.message,
      ' is Mongod not running?/Problem with Atlas Connection?'
    )
  )
  mongoose.connection.on('connected', () =>
    console.log('mongo connected: ', MONGODB_URI)
  )
  mongoose.connection.on('disconnected', () => console.log('mongo disconnected'))

  /*============ MIDDLEWARE =============*/

  // returns data in JSON format:
  app.use(express.json())
  // gives access to static files (all front end files) in the public folder:
  app.use(express.static('public'))

  /*============ CONTROLLERS =============*/

  const controller = require('./controllers/controller.js')
  app.use('/', controller)

  /*============ LISTENER =============*/

  app.listen(PORT, () => {
    console.log("Listening on PORT: " + PORT);
  })
  **********************************************
