---
layout: post
title: "[#D1/30] React30: Getting Started, Basic Concepts"
author: "@codehakase"
---

Today I'm kick starting my 30 day React Article streak on this blog. We are going to start off  Knowing **What React is**, and what makes it thick, setting up our project, and YES create our first React App at the end of this series.

## What is React?
Simply put, React is a JavaScript library for building user interfaces. It is seen as the *View* layer for Web Applicaitons.

React was Created at Facebook to facilitate the creation of interactive, stateful, and resusable UI components.

React applications are broken down into what we call *Components*. A component is a module that renders specific output.

Let's take a *Form* for instance, it consists of many interface elements like buttons, input fields, etc. The various elements can be written as React components, and bind them to a higher-level component, the Form component itself.

## How do we use React?
React is a JavaScript framework. There are two possible ways to run React; Installing on top of Node.js, and using a CDN. In today's article, we're going to Install React on top of Node.js.

### What you'll need
We need a computer, [Node.js](https://nodejs.org/en) installed, and a text editor (I use Sublime Text 3).
Next, open up a terminal and install a cool React scalfolding tool `create-react-app` 
```shell
$ npm install -g create-react-app
```

Now we create a basic React app:
```shell
$ create-react-app react30
```
> react30 is the directory of the app we're creating here

The `create-react-app` tool saves you a lot of time configuring your app from scratch (we're gonna look at this in a later article). We need to change to our Application's directory and run our react app.
```shell 
$ cd react30
$ npm start 
```

It serves the app on `localhost:3000` 

![create-react-app](http://res.cloudinary.com/hakase-labs/image/upload/v1505089587/create-react-app.png "Browser Preview")

We talked about React's basic building blocks, which are components. Let's write one.

From the generated basic app, there's a default component `react30/src/App.js` it looks like this:

```js
class App extends Component {
  render() {
    return (
      <div className="App">
        <div className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <h2>Welcome to React</h2>
        </div>
        <p className="App-intro">
          To get started, edit <code>src/App.js</code> and save to reload.
        </p>
      </div>
    );
  }
}
```

You are probably wondering what JavaScript and HTML are doing at the same place... Don't worry its not Sorcery.. Its called **JSX** - JavaScript XML synaxt transform. This lets you write HTML-like tags inside your JavaScript code. They're very much similar to XML.

The `render` function is the heart of all components. It defines what will be displayed. Let's create a new Component in the `src` directory, we'll call it `AuthorProfile.js` and its content:

```js
import React, { Component } from 'react';

class AuthorProfile extends Component {
  render() {
    return(
      <h1>John Doe - React Ninja!</h1>
    );
  }
}
```

We can now use the newly created component inside of our higher-level component `App.js` 

```js
import React, { Component } from 'react'
import AuthorProfile from './AuthorProfile'

class App extends Component {
  <div className="App">
    <div className="App-header">
      <AuthorProfile />
    </div>
  </div>
}
```

## Props 
We can add attributes to components called props. These attributes are available in our component as `this.props` and can be used in the `render` method. 

Let's modify our `AuthorProfile` component to make use of props, and pass the values of the props from its parent Component.

```js
// AuthorProfile.js

import React, { Component } from 'react';

class AuthorProfile extends Component {
  render() {
    return(
      <h1>{this.props.name} - {this.props.role}!</h1>
    );
  }
}

// App.js
class App extends Component {
  render() {
    return(
      <div className="App">
        <div className="App-header">
          <AuthorProfile name={"John Doe"}, role={"React Ninja"} />
        </div>
      </div>
    );
  }
}
```

## State
State in React enables our app to switch between places. State retains data in our various Components when used.

Every component has a `state` and `prop` object. To set a state, we use the `setState` method.

To use states in a React component, we'll need to add a constructor. Back to our basic app, lets create a form field that updates on the fly using states.

```js
// App.js

class App extends Component {
  constructor() {
    super();
    this.state = {
      txt = "Hello from React!"
    }
  }
 
 update(e) {
   this.setState( {text: e.target.value })
 }
 render() {
  return(
    <div className="App">
      <div className="App-header">
        <AuthorProfile name={"John Doe"}, role={"React Ninja"} />
      </div>
    </div>
   );
 }
}
```

## Source 
The source for this series, can be found [HERE](https://github.com/codehakase/react30-source). You can clone the GitHub repo, and run locally, more instructions are on the README file.


## Conclusion
We've reviewed some React basics in today's article. You should now know what React is, how to get started setting up React locally, what components are, states and props.

In the next installment of React30 we will look at `Lifecycle Methods and Data Fetching`, And also integrating with Express to build an app that renders on the server side, as well as the client side.

Stay tuned!