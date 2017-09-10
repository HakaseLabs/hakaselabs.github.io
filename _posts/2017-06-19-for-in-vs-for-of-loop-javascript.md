---
title: "for...in vs for...of Loops in JavaScript"
layout: post
author: "@codehakase"
---
![for-in-graphics](https://user-images.githubusercontent.com/9336187/27305938-97169192-553b-11e7-99cb-99396a9593fd.png "Hakase Labs")

The `for..in` and `for..of` loops, gives us a clean and concise syntax to iterate on iterable items like arrays, strings, objects, and enumerables. Now the question is where to use which.
Here's a little reminder to get you you started.

## for..in
Use this to iterate over the properties of an object:
```javascript
let person = {
    name: 'Francis',
    alias: 'codehakase',
    eyeColour: 'brown'
};
for(let key in person) {
    console.log( `${key} => ${person[key]}` );
}

// name => Francis
// alias => codehakase
...
```

The `for..in` loop can also be used to iterate over indexed values of a string:
```javascript
let text = 'JavaScript';
for (let i in text) {
    console.log(`Index of ${str[index]}: ${i}`);
}
// Index of J: 0
// Index of a: 1
...
```

# for..of
Use this to iterate over the values in an iterable like an array:
```javascript
let girls = ['Jane', 'Lilian', 'Tobi'];
let boys = ['John', 'Tom', 'Kelvin'];
for (let crush of girl_crush) {
    // lucky crush for them boys
    let index = Math.floor(Math.random() * names.length);
    console.log( `${boys[index} crushes on ${girls}` );
}
// Tom crushes on Tobi
// Kelvin crushes on Lilian
// John crushes on Jane
```

The `for..of` loop can also iterate over `maps`, `generators`, and `DOM node collections`

Now you know where and when to use the `for..in` and `for..of` loops.
Did I miss anything? Let me know in comments section.

Cheers!