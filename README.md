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

Once the files are created, we will also create a new file `.env` to store some necessary environment variables. Then copy the following variables into the new file

```
SLACK_BOT_TOKEN=
SLACK_SIGNING_SECRET=
SLACK_APP_TOKEN=
```

Once the variables are created, we open the following `bolt/index.js` file and paste the following code. This code creates a new Bolt application which is what we need.

```
const { App } = require('@slack/bolt')

const config = (expressApp) => {
  const boltApp = new App({
    token: process.env.SLACK_BOT_TOKEN,
    appToken: process.env.SLACK_APP_TOKEN,
  });
};

module.exports = { config };
```

Now we go to the `app.js` file and import the `bolt/index.js` file, once imported we call the config function to create and configure the Bolt application, we must also pass by arguments the instance of our Express.js server, this so that both use the same instance to have only one server and not have problems.

```
const express = require("express");
const bolt = require("./bolt");
require('dotenv').config()

const app = express();
const port = 3000;

bolt.config(app);

app.get("/", (req, res) => {
  res.send("Hello World!");
});

app.listen(port, () => {
  console.log(`myapp is listening on port ${port}`);
});
```

We are already passing our instance of Express to Bolt, now we need to tell Bolt to use this instance of Express to receive https requests instead of using the instance that uses its default framework, for that we will go to the file `bolt/receiver.js` in this file we will import from Bolt the `ExpressReceiver` function, once imported we will call it passing an object with the `app` property where its value is our instance of Express

```
const { ExpressReceiver } = require('@slack/bolt')

const receiver = (expressApp) => {
  return new ExpressReceiver({
    app: expressApp
  });
};

module.exports = receiver;
```

Now that we have our `ExpressReceiver` we need to integrate it to our Bolt application, for that we will go to the `bolt/index.js` file and import our `ExpressReceiver`, once imported we will pass it to the object of our Bolt application in a new property named `receiver`

```
const { App } = require('@slack/bolt')
const receiver = require("./receiver");

const config = (expressApp) => {
  const boltApp = new App({
    receiver: receiver(expressApp),
    token: process.env.SLACK_BOT_TOKEN,
    appToken: process.env.SLACK_APP_TOKEN,
  });
};

module.exports = { config };
```

Done, we have already configured our Bolt application with our Express server, now what remains is to create the Bolt `handlers` for the Slack requests. For this post we will only create the handler for the `Events` and we will add a simple event when a user joins a channel our application will publish a welcome message in the channel. Ok so we go to the `bolt/handlers/events.js` file and copy the following code

```
const events = (boltApp) => {
  // When a user joins the team, send a message in a predefined channel asking them to introduce themselves
  boltApp.event("member_joined_channel", async ({ event, client, logger }) => {
    try {
      logger.info("event info", event);
      const result = await client.chat.postMessage({
        channel: event.channel,
        text: `Welcome to the team, <@${event.user}>! 🎉 You can introduce yourself in this channel.`,
      });
      logger.info("event result", result);
    } catch (error) {
      logger.error(error);
    }
  });
};

module.exports = events;
```

The next step is to import this file in the bolt/handlers/index.js file and call it, for that we copy the following code in that file. In case your Slack app does not only have events, but also has actions, commands, shortcuts, etc. In this file you should import them once they have been created.

```
const events = require("./events");

const handlers = (boltApp) => {
  events(boltApp);
};

module.exports = handlers;
```

Next we have to pass these handlers to our Bolt application to use them, for that we go to the bolt/index.js file and import the handlers into it, since they are imported we need to call the function and pass it our Bolt application

```
const { App } = require('@slack/bolt')
const receiver = require("./receiver");
const handlers = require("./handlers");

const config = (expressApp) => {
  const boltApp = new App({
    receiver: receiver(expressApp),
    token: process.env.SLACK_BOT_TOKEN,
    appToken: process.env.SLACK_APP_TOKEN,
  });

  handlers(boltApp);
};

module.exports = { config };
```

Now we have to add the endpoints for our handlers, for that we go to the `bolt/receiver.js` file and add them in the `endpoints` property, then we also need to add the verification of the requests coming from slack for that we add the `signingSecret` property

```
const { ExpressReceiver } = require('@slack/bolt')

const receiver = (expressApp) => {
  return new ExpressReceiver({
    app: expressApp,
    signingSecret: process.env.SLACK_SIGNING_SECRET,
    endpoints: {
      events: '/slack/events',
    },
  });
};

module.exports = receiver;
```

Finally, we need to add the values for the environment variables, these values are found in [your app management](https://api.slack.com/apps) page of your slack application in the Basic Information tab. Once we get them we paste them into the `.env` file

![Screen Shot 2022-11-06 at 2 52 15 p m](https://user-images.githubusercontent.com/36525675/200194567-bd4d4c5f-2404-48c6-be29-7237568d2d30.png)

We did it! Our server is ready to receive requests from Slack or from any other party, to start receiving we execute the following to start the server and try

```
npm run start
```

We add our slack app to a channel and add a new person to the channel too and it works!

![Screen Shot 2022-11-06 at 3 00 33 p m](https://user-images.githubusercontent.com/36525675/200194902-a79ad41e-52d1-48ea-bd41-8f872952393a.png)

And it also works if we make a request from the browser to the http://localhost:3000/

![Screen Shot 2022-11-06 at 3 05 41 p m](https://user-images.githubusercontent.com/36525675/200195137-178a4dcb-eafd-466d-ad66-fb2b358595f3.png)


