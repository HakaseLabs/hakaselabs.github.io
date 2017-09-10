---
layout: post
title: "JavaScript Async/Await 101"
comments: true
author: "@codehakase"
---
![async/await]({{ site.url }}/assets/aysnc-await.png "Async/await")

Async and Await has been a blessing to most JavaScript Developers. Even while it was on the [Stage 4 proposal](https://github.com/tc39/ecma262/tree/82bebe057c9fca355cfbfeb36be8e42f18c61e94) for ES6, the feature has been warmtly welcomed.

Node.js now Supports async/await since its version 7.6.

## What is async/await?
If this is your first time seeing/hearing of this term, here's it in plain English:
- Its the newest way/pattern of writing asychronous code in JavaScript, asides Promises and callbacks.
- Async/await compared to Promises, are non-blocking
- Async/await makes aysnchronous code appear and behave like synchronous code.
- Aysnc/await cannot be used with plain callbacks


#### Async/Await Vs Promises (Syntax)
Lets write a function that returns a Promise, which resolves with some data object. When its called, it logs, and return something:
```javascript
const bookTaxi = () => {
    getAvailableDrivers()
        .then (drivers => {
            console.log(drivers)
            return "done"
        })
}
bookTaxi()

```

Implementing same with async/await:

```javascript
const bookTaxi = async () => {
    console.log(await getAvailableDrivers() )
    return "done"
}

bookTaxi()
```

- The second snippet, has the `async` keyword before it. The `await` keyword can only be used inside functions defined with the `async` keyword. The Async functions returns a promise, and the resolved value of the promise wil be whatever is returned from the function.

- The `await getAvailableDrivers()` inside the `console.log` literal means the call will wait until the promise `getAvailableDrivers()` resolves and prints its value.

> The `async` function declaration defines an asynchronous function, which returns an AsycFunction object.

> The `await` operator is used to wait fro a Promise. It can only be used inside an `async` function - MDN


##### Another Example
A simple `async` function to resolve after a specific time.
```javascript
// this function resolves after 5 seconds
const resolveFive = (x) => {
    return new Promise( resolve => {
        setTimeout( () => {
            resolve(x);
        }, 5000);
    });
}
// an async function
const add = async (x) => {
    let a = resolveFive(30);
    let b = resolveFive(40);
    return x + await a + await b;
}
// testing

add(20).then( value => {
    console.log(value); // prints 90 after 5 seconds.
})
```

### But I like Promises!! :(
Yeah Promises are super awesome, but here are some things that make these guys thick:

#### Error handling
With the Async/wait literals, its possible to handle both synchronous and asynchronous errors with the same constructs, compared to using `try/catch` blocks. Lets compare error handling in Promises vs async/await.
```javascript
// Promises....

const bookTaxi = () => {
    try {
        getAvailableDrivers()
            .then( result => {
                const driversInfo = JSON.parse(result) // this fails and is not tracked
                console.log(driversInfo)
            })
    } catch (err) {
        console.log(err)
    }
}
```
With async/await
```javascript
const bookTaxi = async () => {
    try {
        // this may fail
        const driversInfo = JSON.parse(await getAvailableDrivers() )
        console.log(driversInfo)
    } catch(err) {
        console.log(err)
    }
}
```
The difference between both, is in the async/await part, the catch block will handle parsing errors.

#### Clean code
We can write much cleaner code (compare the examples from before). Unlike Promises, we didn't have to use `.then()` or create anonymous functions to handle responses, etc. This also makes one avoid nested Promises, which can sometimes be a mess.

#### Error Stacks
If you call multiple promises in a chain, and an error is thrown somewhere in the chain, the error stack gives no clue of where such error occured.
```javascript
// Promise chain
const bookTaxi = () => {
    return checkRides()
        .then( () => anotherPromise() )
        .then( () => anotherPromise() )
        .then( () => anotherPromise() )
        // ...
        .then( () => {
            throw new Error("something went wrong from our end :/");
        })
}
bookTaxi()
    .catch( err => {
        console.log(err)
    })
```
When an error occures from the above, the stack trace is a disaster, you get something like: `Errpr: something went wrong from our end :/ at anotherPromise.then.then.then.then (file.js: x:y)`

The error stack from async/await, points to the function that contains the error:
```javascript
const booxTaxi = async () => {
    await anotherPromise()
    await anotherPromise()
    await anotherPromise()
    await anotherPromise()
    throw new Error("something went wrong :/");
}
bookTaxi()
    .catch( err => {
        console.log(err); // Error: something went wrong :/ at (file.js: x:x )
    })

```

#### Conditionals
You may have code that fetches some data and decides whether to return that or get more details based on the value of that data.
```javascript
// Promise
const bookTaxi = () => {
    return getRides()
        .then( rides => {
            if ( rides.alreadyBooked ) {
                return getNoneBooked(rides)
                    .then( moreRides => {
                        console.log(moreRides)
                        return moreRides
                    })
            } else {
                console.log(rides)
                return rides
            }
        })
}
```
The above looks a bit messy, with all the nesting and unstable conditionals. The async/await version is much more readable.
```javascript
const bookTaxi = async () => {
    const rides = await getRides()
    if ( rides.alreadyBooked ) {
        const moreRides = await getNoneBooked(rides)
        console.log(moreRides)
        return moreRides
    } else {
        console.log(rides)
        return rides
    }
}
```

### Conclusion
We've seen some of the ups of this feature, and how it can substitute for Promises.

The async/await feature is one of the best that has been added to JavaScript. There are less syntactical mess met compared to using Promises, and debugging is far more shorter and less tiring.

Did I miss anything? Let me know in the comments :)