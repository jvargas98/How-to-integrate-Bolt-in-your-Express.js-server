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

First we will be creating a simple Express.js project and a simple Slack Application.

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

Now run your server to test that everything is OK. Run the next command

```
npm run start
```

If everything is ok you should see the following message
`myapp is listening on port 3000`

### Steps to create your Slack Application

To create your slack application we first need to have a Slack account if you don't have one please create one. In case you already have one you can skip this step. Well to create an account we need to go to the following link

https://slack.com/get-started#/createnew

![Screen Shot 2022-11-05 at 9 13 33 p m](https://user-images.githubusercontent.com/36525675/200152212-384995c6-e09e-4ce8-9526-32a95356cf9c.png)

Now we need to create a workspace, to do that we go to the following link and click on create

https://slack.com/get-started#/landing

![Screen Shot 2022-11-05 at 9 16 46 p m](https://user-images.githubusercontent.com/36525675/200152453-b4dd0b1f-5aeb-48ff-ad97-c24d9829f8aa.png)

Complete all steps to create the workspace

![Screen Shot 2022-11-05 at 9 35 34 p m](https://user-images.githubusercontent.com/36525675/200153649-d8d0cafc-3084-466b-9670-9faa436193e7.png)

Done! We have created a workspace correctly 

![Screen Shot 2022-11-05 at 9 36 49 p m](https://user-images.githubusercontent.com/36525675/200153655-b3b18288-8b70-4ced-b803-6dcb2743d0c0.png)


