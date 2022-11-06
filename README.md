# How to integrate Bolt in your Express.js server

## About

In this post I will show you how to integrate the Bolt for JavaScript framework to your Express.js server to make your Slack application can coexist with your server, this to have both features of both frameworks in a single server.

## What is Bolt?
Bolt is a foundational framework that makes it easier to build Slack apps with the platform's latest features.

Bolt handles much of the foundational setup so you can focus on your app's functionality. Out of the box, Bolt includes:

  - A basic web server to run your app on
  - Authentication and installation handling for all the ins and outs of OAuth
  - Simplified interfaces for all Slack APIs and app features
  - Automatic token validation, retry, and rate-limiting logic
  - Bolt also has built-in type support, so you can get more work done right from your code editor.

Bolt is available for JavaScript, Python and Java.

As I mentioned Bolt for JavaScript is a Node.js framework just like Express.js. So if we want to integrate both frameworks in a single Node.js server is not possible because both create an independent instance of a Node.js server by logic could not coexist on the same server but fortunately Bolt has a feature which allows us to integrate it together with Express.js

## Let's get started

First we need to have an Express project, if you don't have one then let's create it, in case you already have one you can skip this step.

### Steps to create your Express server

Assuming you already have Node.js installed, create a directory to hold your application, for this please open a terminal and type the next commands

```
mkdir myapp
cd myapp
```

Create a `package.json` file for your application with the next command

```
npm init
```

Then we install Express

```
npm install express
```

Now we create a new file with the name `app.js` and paste the following code

```
const express = require('express')
const app = express()
const port = 3000

app.get('/', (req, res) => {
  res.send('Hello World!')
})

app.listen(port, () => {
  console.log(`myapp is listening on port ${port}`)
})
```

Create a new script to start the server by adding the following line inside the scripts property in the `package.json` file
```
"scripts": {
    "start": "node app.js"
},
```

Start your server to test that everything is OK. To start your server run the next command

```
npm run start
```

If everything is ok you should see the following message
`myapp is listening on port 3000`

## Steps to setup Bolt in your Express.js server

The first step is to install Bolt, for that we execute the following command

```
npm install @slack/bolt
```

Then, we replicate the following structure for the bolt folder, please create the necessary folders and files

![Screen Shot 2022-11-06 at 1 01 07 p m](https://user-images.githubusercontent.com/36525675/200189929-b9472035-328f-4b3f-b134-2695319cc02d.png)


