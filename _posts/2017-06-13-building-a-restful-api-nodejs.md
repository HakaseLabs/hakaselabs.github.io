---
title: "Build your first RESTful API with Node.js"
layout: post
author: "@codehakase"
tags: "node.js, api, rest, node"
---
![Node.js](https://softwareengineeringdaily.com/wp-content/uploads/2015/08/nodejs_logo_green.jpg "Node.js")

**Node.js** is one intimidating JavaScript framework, especially for beginners. This article serves as a quick quide to Node.js, Express.js and MongoDB. We'll building a simple REST API that'll serve as a basic foundation for an application.

For the purpose of this tutorial, you'll be creating the base for a *ToDo List* application (yeah its kinda like the convention to start with ToDo list apps). You'll use all CRUD (create, read, update and delete) actions on the API.

# What is REST?
REST is an acronym for Representational State Transfer. It is a web standards architecture and HTTP Protocol. The REST protocol, decribes six (6) constraints:
1. Uniform Interface
2. Cacheable
3. Client-Server
4. Stateless
5. Code on Demand
6. Layered System

REST is composed of methods such as a base URL, media types, etc. RESTful applicaitons uses HTTP requests to perform the CRUD operations.


# Setup
You'll need to have Node installed, if not in place get in [here](https://howtonode.org/how-to-install-nodejs).

I'm on Elementary OS (an Ubuntu flavoured distro), so i'll be using the command line alot.

In a new directory `todoApp` or whatever you'd like to name yours, we'll need a `package.json` file, so we'll use `npm` to generate one:
```shell
npm init
```
Feel free to use or change the defaults. Once that's done, you should be a `package.json` file generated in the root directory.

We need Express as our web framework, `body-parser` to help handle JSON requests, MongoDB for the database, and Nodemon to watch for file changes while our app is being served.
```bash
npm install --save express body-parser mongoose && npm install --save-dev nodemon
```
> * You may need root access for npm to write out files, just append `sudo` to the command to grant root access.
> * We'll be using Mongoose to interact with a MongoDB instance, you'll still need to have MongoDB server installed on your machine

After the package installation is done, your complete `package.json` file should look like this:
```json
{
  "name": "todoApp",
  "version": "0.0.0",
  "description": "Building a REST API with Node.js",

  "author": "Francis Sunday",
  "license": "MIT",
  "dependencies": {
    "body-parser": "^1.17.2",
    "express": "^4.15.3",
    "mongoose": "^4.10.4"
  },
  "devDependencies": {
    "nodemon": "^1.11.0"
  }
}

```

## Serving up the App
If you check your directory you'd notice a new folder `node_modules` that's where the dependencies were saved by the `npm install` command. We need to require them and start up our app. Go ahead and create a `server.js` file with this content:
```javascript
const express = require('express');
const app = express();
const port = process.env.PORT || 3000;
const mongoose = require('mongoose');
const bodyParser = require('body-parser');

// get our server running
app.listen(port, () => {
    console.log("App up and running on" + port);
});
```

Now run `node server.js` and you'd see the log.

## File Structure
We need to structure or directory so we have dedicated files for various actions - `routes, models,` and `controllers`. Go ahead and create the following directory structure:
```txt
--todoApp
    - api
        - models
        - controllers
        - routes
    - node_modules
    - server.js
    - package.json
    ...
```
We create the dedicated files in their directories-
`
api/models/todoModel.js
api/controllers/todoController.js
api/routes/todoRoutes.js
`

Your directory structure should look like this now:
```txt
--todoApp
    - api
        - models
            - todoModel.js
        - controllers
            - todoController.js
        - routes
            - todoRoutes.js
    - node_modules
    - server.js
    - package.json
    ...
```

### CRUD Routes
For this api, you'll create four (4) different routes, to handle CREAting a todo item, READing an item, UPDATE and item, and DELETE an item.
You're also gonna need to test your API while developing, so we'll use an awesome app called [Postman](https://getpostman.com), which will allow us make simple HTTP requests.

#### Your First Route
Routing refers to determining how an application responds to requests, which is a URI and a specific request method (POST, GET, DELETE, etc).

We're gonna define two basic routes `/tasks` and `/tasks/{taskid}`
```javascript
// api/models/todoRoutes.js

module.exports = (app) => {
    let todoList = require('../controllers/todoController');

    // our Routes
    app.route('/tasks')
        .get(todoList.getTasks)
        .post(todoList.createTask);

    app.route('/tasks/:taskId')
        .get(todoList.readTask)
        .put(todoList.updateTask)
        .delete(todoList.deleteTask);
}
```
In the code above, the API's routes are defined under different verbs; when a request is made for the `tasks` route e.g `todoApp.dev/tasks`, it calls the `getTasks` method from the required todoList controller, same for the `post`, `put`, and `delete` routes. The `tasks/:taskId` route, handles a single task. We can grab a task via its ID - `/tasks/3` update or delete it too.

## Database Schema
We'll be using Mongoose to interact with a MongoDB instance. In the `todoModel.js` file, we'll define a schema for our Tasks collection. With Mongoose, we can create Schemas easily by defining the fields and their types.
> You'll need to have MongoDB server installed locally, if you want to serve your database. You can also use a remote database, there's a free tier from [MLab]('https://mlab.com)

```javascript
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

let TaskSchema = new Schema({
    name: {
        type: String,
        Required: 'Task label is required!'
    },
    Created_date: {
        type: Date,
        default: Date.now
    },
    status: {
        type: [{
            type: String,
            enum: ['completed', 'ongoing', 'pending']
        }],
        default: ['pending']
    }
});

module.exports = mongoose.model('Tasks', TaskSchema);
```

In the `taskModel` file, we created a schema for it. As you can see, it the task collection(table) will contain a name: a string, and the date it was created.

### Setting up Controllers
Remember those methods attached to each verb in the `todoRoutes.js` file, they are controller methods, and we are creating them in the `api/controllers/todoController.js` file:

```javascript
const mongoose = require("mongoose");
const Task = mongoose.model("Tasks");

// get all tasks

exports.getTasks = (req, res) => {
    Task.find({}, (err, task) => {
        if (err)
            res.send(err);

        res.json(task);
    });
};

// create a task

exports.createTask = (req, res) => {
    let newTask = new Task(req.body);
    newTask.save( (err, task) => {
        if (err)
            res.send(err);

        res.json(task);
    });
};

// read a single task

exports.readTask = (req, res) => {
    Task.findById(req.params.id, (err, task) => {
        if (err)
            res.send(err);

        res.json(task);
    });
};

// update a particular task

exports.updateTask = (req, res) => {
  Task.findOneAndUpdate(req.params.id, req.body, { new: true }, (err, task) => {
    if (err)
        res.send(err);

    res.json(task);
  });
};

// delete a single task

exports.deleteTask = (req, res) => {
    Task.remove({
        _id: req.params.id
    }, (err, task) => {
        if (err)
            res.send(err);
        res.json({ message: 'Task deleted!!' });
    });
};
```

### Coupling everything
Back in our `server.js` file, we'll connect to our database, by adding a URL to the mongoose connection instance, Load the created Model (`task`), register our created routes.
Update your `server.js` file to look like this:

```javascript
const express = require("express"); // express framework
const app = express();
const port = process.env.PORT || 3000;
const mongoose = require("mongoose");
const Task = require("./api/models/todoListModel");
const bodyParser = require('body-parser');

mongoose.Promise = global.Promise;
mongoose.connect("mongodb://localhost:2701/todoApp"); // connect to MongoDB

// handle incoming requests
app.use(bodyParser.urlencoded({ extended: true }));
app.use(bodyParser.json());

// middleware to handle wrong routes

app.use( (req, res) => {
    res.status(404).send({ url: req.originalUrl + 'not found' });
});

let routes = require("./api/routes/todoListRoute");
routes(app); // register our routes

app.listen(port);
console.log('App running on ' + port);

```

## Testing with Postman
To test your API using postman, startup the server `nodemon server.js` and then open up the postman app and pass in your url

#### Get all tasks
![node-api-postman-get](https://user-images.githubusercontent.com/9336187/27061121-6fa2db82-4fd9-11e7-9b51-6116f15da36e.png)

#### Create a Task
![node-api-postman-post](https://user-images.githubusercontent.com/9336187/27061119-6f56e13c-4fd9-11e7-9ebc-5b5afb1aac01.png)
> Note: To use the post method on postman, the *Body* should be set to *x-www-form-urlencode*

[**Update: 2017-06-24** Source code for this tutorial -> [Here](https://github.com/HakaseLabs/source-blog/tree/master/rest-api-nodejs)]

## That's it!
You have a working Node.js API which makes use of the four major HTTP verbs (GET, POST, PUT, DELETE).

This tutorial was created to give you a familarity with the Node.js development environment along side Express.js and MongoDB.

Was this helpful? feel free to leave a comment below. Also, if you have questions, noticed an error, please do well to let me know.

Thanks!