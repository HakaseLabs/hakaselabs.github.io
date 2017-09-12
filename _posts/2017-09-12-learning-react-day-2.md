---
layout: post
title: "[#D2/30] React30: Lifecycle Methods and Data Fetching"
author: "@codehakase"
---

In the [previous article](https://hakaselabs.github.io/2017-09-11/learning-react-day-1), I wrote on setting up React, and some other basic features like Components, Props, and States. In this article, we're going to look at React's Lifecycle Methods and how to use them.

When creating components, the only required spec is the `render` method. Sometimes we may want to add code that gets called at certain times inside our component's lifetime. The Lifecycle methods in React allows us write additional code for this.

## Lifecycle Methods
- `componentWillMount` - Invoked once, on both client and server before its been rendered.
- `componentDidMount`  - Invoked once, only on the client, after a component is rendered.
- `shouldComponentUpdate` - Determines whether a compoennt should update itself.
- `componentWillUnmount` - Invoked when a component is unmounted.

### componentDidMount
In this article, we're going to add to our basic app from the previous article ([GitHub repo](https://github.com/codehakase/react30-source)). We are going to make an API call to a Weather service.

We're going to create a new Component `WeatherDisplay` and use it in the `App` component.

In the `/src` directory, we create a new file `WeatherDisplay.js` with this content:
```js
import React, { Component } from 'react';

class WeatherDisplay extends Component {
  constructor() {
    super();
    this.state = {
      weatherData: null
    }
  }

  componentDidMount() {
    const zipCode = this.props.zipCode;
    const URL = "http://api.openweathermap.org/data/2.5/weather?q=" +
      zipCode +
      "&appid=b1b35bba8b434a28a0be2a3e1071ae5b&units=imperial";
     fetch(URL).then(res => res.json()).then(json => {
      this.setState({
        weatherData: json
      });
     });
  }

  render() {
    const weatherData = this.state.weatherData;
    if (!weatherData) return <div>Loading</div>;
    const weather = weatherData.weather[0];
    const icon = "http://openweathermap.org/img/w/" + weather.icon + ".png";

    return (
      <div>
        <h1>
          {weather.main} in {weatherData.name}
          <img src={icon} alt={weatherData.description} />
        </h1>
        <p>Current: {weatherData.main.temp}°</p>
        <p>High: {weatherData.main.temp_max}°</p>
        <p>Low: {weatherData.main.temp_min}°</p>
        <p>Wind Speed: {weatherData.wind.speed} mi/hr</p>
      </div>
    );
  }
}

export default WeatherDisplay; 
```

And update `App.js` with this content:
```js
import React, { Component } from 'react';
import './App.css';
import WeatherDisplay from './WeatherDisplay';

class App extends Component {
  constructor() {
    super();
    this.state = {
      activePlace: 0
    };
  }
  render() {
    const places = [
      {name: "Asokoro", zipCode: "900231"},
      {name: "Maitama", zipCode: "900271"},
      { name: "Santa Cruz", zipCode: "95062" },
      { name: "Honolulu", zipCode: "96803" }
    ];

    const activePlace = this.state.activePlace;
    return (
      <div className="App">
        {
          places.map((place, index) => (
            <button
              key={index}
              onClick={() => {this.setState({
                activePlace: index
              });
             }}
             >
             {place.name}
            </button>
          ))
        }
        <WeatherDisplay 
          key={activePlace}
          zipCode={places[activePlace].zipCode}
          />
      </div>
    );
  }
}

export default App;
```

At this point running your app on you should get something similar:

![React basic app](http://res.cloudinary.com/hakase-labs/image/upload/v1505241392/react30-basic-app_vghmhe.gif)


## Source
The source for this series, can be found [HERE](https://github.com/codehakase/react30-source). You can clone the GitHub repo, and run locally, more instructions are on the README file.


## Conclusion 
So far we've seen some of the React Lifecycle hooks, and a sample app to explain further. When we talk about mounting, we're actually referring to the process of converting components into actual DOM elements. In the next article, we'll look at the other Lifecycle methods in-depth.

### Read other articles in the series 
Day 1 - [Getting Started, the Basics](https://hakaselabs.github.io/2017-09-11/learning-react-day-1)
