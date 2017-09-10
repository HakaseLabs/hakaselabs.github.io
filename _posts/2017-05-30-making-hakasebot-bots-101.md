---
layout: post
author: "@codehakase"
title: "Making hakasebot - Twitter Bots 101"
comments: true
---
![Bots]({{ site.url }}/assets/twitter-bots.gif "Bots")  
 
  
This was my first attempt making twitter bots. I made a very simple twitter bot for this blog, check the [Source](https://github.com/codehakase/hakasebot) and also follow [@_hakasebot](https://twitter.com/_hakasebot).

## Setting Up
The bot was created using the [Twit](https://github.com/ttezel/twit) package, which is a Twitter API client for Node.js. Twit needs to connect with my twitter account so first I created a new [Twitter Application](https://apps.twitter.com/). After that, I took note of my application's keys:
* Consumer Key 
* Consumer Secret
* Access Token
* Access Token Secret

You can find these keys on the **Keys and Access Tokens** panel in you app's dashboard.

Once these keys are all ready, we create a new Node.js project and initialise the Twit package. 
> Prerequisites: Node.js, npm, and of course a PC 

So you can create a directory and create three  files `package.json`, `config.js`,  and `bot.js`

In the `config.js` file, we setup Twit.
```javascript
//config.js
const Twit = require('twit');
const TH = new Twit({ // Twit Handler
    consumer_key: APPLICATION_CONSUMER_KEY_HERE,
    consumer_secret: APPLICATION_CONSUMER_SECRET_HERE,
    access_token: ACCESS_TOKEN_HERE,
    access_token_secret: ACCESS_TOKEN_SECRET_HERE
});
```
Basically, @_hakasebot does the following:
* Listens for Events and keywords
* Responds to Events
    * Like
    * Retweet
    * Reply

## Listens for Events and Keywords
Twitter has a [Streaming API](https://dev.twitter.com/streaming/overview), which gives us access to the streams of tweets. @_hakasebot uses two streams from the API:
* The **User Stream**, which is a stream of tweets corresponding to a single user.
* The **Public Stream**, which is the stream of all public tweets.

With the *public* stream, @_hakasebot can listen for tweets from any users that contains a defined keyword. This is possible when we create a stream of tweets based on a filter suing the **statuses/filter** endpoint, and passing an object with the filter parameters. The **track** parameter is used to filter tweets by keyword, and it accepts a string or an array of keywords to lookout for. 

@_hakasebot runs on a filter that searches for mentions of this blog, so we'd implement as so:

```javascript
const stream = TH.stream('statuses/filter', {
    track: ['hakasebot', 'hakaselabs', 'hakaselabs.github.io']
});
```
When a stream if open, we can listen and respond to tweets that falls within that stream.
```javascript
stream.on('tweet', (tweet) => {
    // We do something with that tweet here
});
```

## Responding to Events
We can respond to events by posting a tweet, retweeting, replying, follow a user, etc. 
@_hakasebot is able to take three actions currently - like, reply, and retweet.

### Liking a tweet
if the tweet was from another account, the bot likes it. To like a tweet, we post to the **/favourites/create** endpoint, passing the id of the tweet to be favorited.
```javascript
stream.on('tweet', (tweet) => {
    if (tweet.user.id == _self.id) { // 
        // we'll get back to this 
    }
    TH.post('favourites/create', {
        id: tweet.id_str
    });
});

```
### Replying a tweet
If the tweet was from another user, the bot sends them a reply. We send a reply by posting to the **/statuses/update** endpoint and passing the id of the tweet we are replying to.
```javascript
stream.on('tweet', (tweet) => {
    if (tweet.user.id == _self.id) {....}

    TH.post('favourites/create', {
        id: tweet.id_str
    });
    TH.post('statuses/update', {
        status: `@${tweet.user.screen_name} Thanks for sharing :)`,
        in_reply_to_status_id: tweet.id_str 
    });
});
```
### Retweeting
@_hakasebot retweets a tweet if it is found from my stream - That means if the tweet found from the stream is from myself [@codehakase](https://twitter.com/codehakase), it retweets it. We can retweet by posting to the **/statuses/retweet/:id** endpoint.
```javascript
const _self = {
    id: 3354871743,
    screen_name: 'codehakase'
}
...

stream.on('tweet', (tweet) => {
    if (tweet.user.id == _self.id) {
        TH.post('statuses/retweet/:id', {
            id: tweet.id_str
        });
        return;
    }
    ....
});
```
> You can get your twitter account id from [here](http://gettwitterid.com)



## Deploying the Bot
I used [Heroku](http://heroku.com) to host @_hakasebot. Since its a Node.js app, we need to place some information in our `package.json` file:
* The main script - The file Node.js would run
* Dependencies
* The version of Node.js


My `package.json` file looks this way:
```json
{
  "name": "hakasebot",
  "version": "1.0.0",
  "description": "A twitter bot for hakaselabs.github.io",
  "main": "bot.js",
  "scripts": {
    "start": "node bot.js"
  },
  "author": "Francis Sunday - codehakase",

  "dependencies": {
    "twit": "^2.2.5",
    "express": "^4.14.0"
  },
  "engines": {
    "node": "7.9.0"
  }
}
```
Great! you now have an idea of how to make bots for Twitter. You can follow [@_hakasebot](https://twitter.com/_hakasebot), view the [source](https://github.com/codehakase/hakasebot), to test, use the twitter share button below. 

Have you built bots for Twitter? I'd like to know, drop them in the comment section below.