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

![Screen Shot 2022-11-05 at 10 15 14 p m](https://user-images.githubusercontent.com/36525675/200153886-0efb46e3-5eb0-4ac5-9e79-e1da5c35ab70.png)

The next step is to create the Slack application. To create it we have to go to the following link

https://api.slack.com/apps

Click on the button `Create an App`

![Screen Shot 2022-11-05 at 10 19 48 p m](https://user-images.githubusercontent.com/36525675/200154055-0528049e-670d-44e8-808d-e4eb2c72d573.png)

After clicking on the button, the following window will appear, where we will select the last option that says 

![Screen Shot 2022-11-05 at 10 33 33 p m](https://user-images.githubusercontent.com/36525675/200154514-8662e837-95e0-4d97-b414-c7ea9a639f47.png)
![Screen Shot 2022-11-05 at 10 34 00 p m](https://user-images.githubusercontent.com/36525675/200154516-00a90306-5d6f-4ca5-9c42-753a6f519962.png)
