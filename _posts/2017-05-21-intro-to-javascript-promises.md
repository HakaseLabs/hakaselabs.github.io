---
title: Introduction to JavaScript Promises
layout: post
comments: true
author: "@codehakase"
---

JavaScript promises have become a popular way to handle the tangled mess that JavaScript’s asynchronous nature often creates for us. 
Synchronous code is eaiser to follow and debug, async is better for flexibiity. Promises are becomming a big part of the JavaScript world, with awesome APIs implemented with it.

## What is a Promise?

A Promise is a proxy for a value not necessarily known when the promise is created. It allows you to associate handlers with an asynchronous action's eventual success value or failure reason. This lets asynchronous methods return values like synchronous methods: instead of immediately returning the final value, the asynchronous method returns a promise to supply the value at some point in the future. - **Mozilla Developer Network (MDN)**


## How Promises work
A promise, returns an object synchronously from asynchronous function, in three possible states:
* Fuilfiled `resolve()` - The action relating to the promise succeeded
* Rejected `reject()` - The action relating to the promise failed
* Pending - not yet fulfilled or rejected
* Settled - Has fulfilled or rejected

**A simple promise:** The code below has a function that returns a promise after a specified time delay.

```javascript
const greet = t => new Promise((resolve) => setTimeout(resolve, t));

greet(5000)
    .then( () => console.log("Promises!!"));
```
The `greet` function waits for 5 seconds (5000ms) and log to the console a string.

## Basic Promise Usage
A new Promise is created with the `new` keyword and the promise provides `resolve` and `reject` functions to the provided callback:

```javascript
const myPromise = new Promise( (resolve, reject) => {
    // we could run an async task here
    if (true) { // condition is good
        resolve('Success!!');
    } else {;
        reject('Failure')
    }

    myPromise.then( () => {
        // do something with the result
    }).catch(function() {
        // error message
    })
});
```
The ES6 promise constructor takes a function, with two parameters, `resolve()` and `reject()`. 

### then()
All promise instance, gets a `then` method which allows you to react to the promise. It receives the result given to it by the `resolve()` call. 
The `then` callback method, is triggered when a promise is resolved.
```javascript
const p = new Promise( (resolve, reject) {
    setTimeout( () => {
        resolve(10);
    }, 3000);
})
.then( (num) => {
    console.log('then call 1: ', num);
    return num * 2;
})
.then( (num) => {
    console.log('then call 2: ', num);
    return num * 2;
})
```
The output of the above code:
```
    > then call 1: 10
    > then call 2: 20
```
### catch()
The `catch` callback is executed when a promise is rejected

```javascript
const p = new Promise( (resolve, reject) => {
    setTimeout(function() {
        reject('Rejected!!');
    }, 2000);
})
.then( (e) => { console.log('done', e); })
.catch( (e) => { console.log('catch:', e); } )

// output
// catch: Rejected!!
```
You can also use Promises in jQuery. jQuery has an object called `Deferred` that has `resolve` and `reject` methods that behaves like the resolve and reject methods passed to the Native JavaScript Promise constructor.

```javascript
//using jQuery 
let p = $.Deferred(function (dfd) {
    setTimeout(dfd.resolve, 1000);
}).p();
```
## Important Promise Rules
A standard for promises was defined by the [Promises/A+ specification](https://promisesaplus.com/implementations) community. 
Promises following the specmust follow a specific set of rules:
* A promise, is an object that supplies a standard compliant `.then` method.
* A pending promise may transition into a fulfilled or rejected state.
* A fulfiled or rejected promise is settled, and must not transition into any other state.
* Once a promise is settled, it must have a value, which must not change.
```
The change refers to identity (===) comparison
```
## Extras
The native `Promise` object has some extra stuff you might be interested in:
* `Promise.reject()` return a rejected promise.
* `Promise.resolve()` returns a resolved promise.
*  `Promise.race()` takes an array and returns a promise that resolves with the value of the resolved promise in th eiterable or rejects with the first promise that rejects.
* `Promise.all()` take and array, and returns a promise that resolves when all the promises in te iterable argument have resolved, or rejects with the reason of the first passed promise that rejects.
## Browser support & Polyfill
As of Chrome 32, Firefox 29, Safari 8 & Microsoft Edge, promises are enabled by default.
To bring browsers that lack a complete promises implementation, there's [Polyfill](https://github.com/jakearchibald/ES6-Promises#readme) to help out.

## Conclusion
Promises have become an integral part of JavaScript and a very powerful way of managing asychronous behavior in JavaScript. By next week, I'll publish a read on the Properties of Promises, and relate to a live sample.
Promises takes some time to get used used too (I still find it confusing at some point), so to grasp this concept, you have to *Learn, Un-learn, and Re-learn*.
Have any questions or comments? Leave them below :bowtie: .

## References
[Mozilla Developer Network](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Promise)

[Jake Archibald - Google Developers](https://developers.google.com/web/fundamentals/getting-started/primers/promises)
