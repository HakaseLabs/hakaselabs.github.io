---
title: "JavaScript Objects 101"
layout: post
author: "@codehakase"
---
In JavaScript, most things are objects, from core JavaScript features like strings and arrays to the browser APIs built on top of JavaScript. You can even create your own objects to encapsulate related functions and variables into efficient packages, and act as handy data containers.

### What Is An Object?
In JavaScript terms, An Object is a collection of data, which consits of several variables and functions - which are called properties and methods.

Creating an object is as easy as eating candy (lol!). Taking our Vehicle instance, we can create a Car Object like so:
```javascript
let car = {
    type: "salon",
    colour: "black",
}
```

In the code above, we created a variable `car` which is now an object, and has some `key-value` pairs inside. The `type` and `colour` items inside our car object are referred to as properties, and they hold their various values. 
If we log to our console the type of our car variable, we'd get an object `console.log(typeof car); //object` .

> Note: We separate each key-value pairs with a comma (,)

#### Accessing Object Properties
Now we've created our object, how do we access the items inside? Its easy. We use the `dot` notation to access properties of an object. Say we want to log to our console, the value of the `colour` property in our `car` object, we'll write as so:
```javascript
console.log( car.colour ); // black
```
#### What Data Types are allowed as Property values in Objects?
An Object's property can have all data type as its value (`strings, number, float, array, function, ...`).
```javascript
let me = {
    name: "Francis Sunday",
    gender: "male",
    interests: ['God', 'programming', 'music', 'writing'],
    bio: function() {
        console.log( "Hi! I'm " + this.name + ", and I'm interested in " + this.showInterests() )
    },
    showInterests: function() {
        return this.interests.join(", ");
    }
}
```
Quite a lot going on up there, nothing to worry, I'll walk you through it. 
First I created a variable `me` to hold an object holding the properties `name, interests, gender`, and methods `bio(), showInterests()`. The `this` keyword here, refers to the current object. So `this.name` also means the value of the `name` property in this particular object I'm in.

Let's use the object.
```javascript
console.log ( me.name ); // "Francis Sunday"
console.log ( me.gender ); // male
console.log ( me.interests ) // ['God', 'programming' ...]
me.bio(); //  Hi! I'm Francis Sunday, and I'm interested in God, programming, music, writing
```
Now you get the feel of objects :+1:

### Objects Are Everywhere
You need to grab an element in a DIV with html, you'd do something like this:
```javascript
let myDiv = document.getElementById('thatDiv');
``` 
`document` is an object and `getElementById` is a method of it. You get the idea now right?

Here are some other examples of objects:
```javascript
someVar.split('');
someVar.join(',');
someVar.style
...
```

#### Sub-Namespaces
Values of objects, can also become objects. And they are still accessed via the dot notation.
If i change the name property in the `me` object as so:
```javascript
name: {
    firstname: "Francis",
    lastname: "Sunday"
}
```
To access those inner objects, we just extend with another `dot`.
```javascript
me.name.firstname // Francis
me.name.lastname // Sunday

```

#### Updating Objects Properties
After an Object has been created with its properties, we can still update the values of its properties.
```javascript
let person = {
    name: "John Doe",
    age: 19
}
// changing the value of the name property
console.log ( person.name ); // John Doe
person.name = "Brian";
console.log( person.name ); // Brian
```

Also, we can create new properties outside the creation block.
```javascript
let car = {
    model: "MV400",
    colour: "blue"
}

car.name = "benz";

console.log( car ); 
// Output 
// { model: 'MV400', colour: 'blue', name: 'benz' }
```

### Try It
We've covered a lot of ground so far, to make sure everything we learned so far sinks in, open up your favourite editor (mine is VSCode), and try a quick example.

Create a basic `HTML` markup with two text fields `name`, `age` and a `submit` button. Make sure all the form fields has an ID.
In your JavaScript file, grab the items from the text fields when the submit button is clicked and add the values of the form fields to a `userInfo` object.
Display the inputs from the text fields to the browser in a `div`.

I'll wait here for you. No peeking.

---
Okay, if you didn't peek, you should have something like this:
```html
<!--the HTML markup-->
<input type="text" id="name" placeholder="Enter Your Name" >
<input type="text" id="age" placeholder="Enter Your Age" >
<button id="submitBtn">Submit</button>

<!--for the output-->
<div id="info"></div>

```

```javascript
// The JavaScript code
let _name = document.getElementById('name'); 
let _age = document.getElementById('age');
let btn = document.getElementById('submitBtn');

// when the send button is clicked
btn.addEventListener('click', function() {
// The userInfo object, holding the values from the text fields
let userInfo = {
    name: _name.value,
    age: _age.value,
    display: function() {
        return "Name: " + this.name + ", " + "Age: " + this.age;
    }
}
// displays the data in the HTML markup
document.getElementById('info').innerHTML = userInfo.display();
});
```
Preview

![Objects-demo]({{site.url}}/assets/Objects-demo.gif "Objects Demo")  

<br/><br/>
<br/><br/>

### Summary
In this title, we walked through the basics of Objects in JavaScript. In the next part (*JavaScript Objects 102*), I'll be writing on Object Oriented programming in JavaScript, with highlights on Prototypes, Inheritance, and Constructors. 
Have any questions? Drop them in the comment box below.
