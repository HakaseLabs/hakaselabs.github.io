---
layout: post
title: "Introducing Adonis Js"
author: "@codehakase"
---

## TL;DR
AdonisJs is a true MVC Framework for Node.js. It encapsulate all the boring parts of Web programming and offers you a nice & clean API to work with. AdonisJs makes it easy to write web applications with less code. In this article, i will show you how to get started with AdonisJs. Checkout the repo on Github.

Node.js is one of the emerging technologies to write real-time applications using one of your favorite web languages: Javascript. With the ample choices of frameworks to write your first web server, not even a single one offers the desired developer experience. This is where AdonisJs shines.

## What really Makes AdonisJs tick?

AdonisJs is inspired by a PHP Framework called Laravel. It borrows the concepts of Dependency injection and service providers to write beautiful code which is testable to its core. Shipped with some cool features like:

- Lucid ORM (Effective Implementation of Active Record)
- Authentication
- Database Migrations
- Mailing System
- Data Validator

Similar to **Laravel**, here are some features Adonis has which are similar to Laravel’s:

- Routing
- Scaffolding
- IOC (Inversion of Control) and Dependency Injection (DI)
- Query Builder
- Model Factories and Database Seeding
- Directory Structure
- Artisan like command utility called ACE

## Why should I use AdonisJs?

AdonisJs focuses on the key aspects of creating stable and scalable web applications, some of them are:

### Developer Experience

The framework makes use of latest inbuilt ES2015 features to get rid of spaghetti code. You will hardly find yourself writing callbacks since there is a solid support for ES2015 generators. Also, the OOP nature of the framework helps you in abstracting your code into multiple re-usable chunks.
Consistent API

### Consistent API
The API throughout the code base is so consistent that after working for a while, you will be able to guess the method names, expected output, etc.
Speed & Productivity

### Speed and Productivity 
AdonisJs ships with a bunch of 1st party components known as providers. Writing entire web server is a matter of weeks(if not days). It ships with solid support for **Mailing, Authentication, Redis, SQL ORM, Data validation and sanitization**, etc.

## Installing AdonisJS

Installing AdonisJs is very straight forward, the prerequisites is Nodejs >=4.0 and NPM >=3.0.

npm is a package manager for Node.js. During the development process, you will find yourself using npm install a lot. Hence all dependencies are pulled from npm only.

*Note:* We’ll be using a command line (terminal) through out, if you are on Mac OS, Ubuntu, Linux the commands we’d use should work. If you are on windows, get a command line tool like Git Bash, so you can use the same comamnds.

### Installing Adonis-CLI

We’ll install adonis-cli globally using npm. It is a command line tool to scaffold new applications within a matter of seconds.

```
npm i -g adonis-cli
```

![](http://i1.wp.com/cdn-images-1.medium.com/max/1600/1*gCIyTiyEis5hUtxdmAAn0A.gif)

With Adonis-CLI installed, we can create projects using it. For this tutorial, we’ll be creating a project holla-adonis.

We’ll create the project using Adonis-CLI

```
adonis new holla-adonis
```

![](http://i0.wp.com/cdn-images-1.medium.com/max/1600/1*k0mjXf6_EclO_MbTpRq5SA.gif) 
scaffolding a new project with adonis-cli

Now you’ve created a new project, lets move into the directory and launch the HTTP server.
```
cd holla-adonis thennpm run serve:dev
```
Now open up your favourite browser and navigate to http://localhost:3333 and you should see the welcome screen.
![welcome screen](http://i0.wp.com/cdn-images-1.medium.com/max/1600/1*xA3sJQFPVwMangNEKs0QRw.png)

## Installing AdonisJs manually

If you’d prefer installing Adonis without adonis-cli (which you should definitely use), you can clone the source from Github and build manually:

```
git clone --dissociate https://github.com/adonisjs/adonis-app holla-adonis

cd holla-adonis
```


### Install Dependencies
```
npm install
```
**Were there problems while Installing AdonisJs?**

If you have not updated to the latest or supported version of Node.js , you might encounter Proxies Error why? Because older versions of Node.js requires the --harmony_proxies flag to add support for ES2015 Proxies. If you are using Node.js < 6.0, make sure to make following changes.

Open up the package.json file in your holla-adonis directory, and replace the values of the json keys to this:

And replace the first line of the ace file to:

    #!/usr/bin/env node — harmony_proxies

### Conclusion

In this article, we explored the AdonisJs framework from noting its features, to Installing and setting up a Project with it.