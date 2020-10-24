#### Steps to make node project

- make new directory
- npm init (answer all the questions) or use `npm init -y` to accept defaults
- create entry point file 'index.js'
- run entry point file with 'node index.js'

#### Node Modules
Node modules are including using the `requires(<module>)` statement. These can be files on your computer, default modules like HTTPServer or fs (filesystem) or 3rd party modules. You can write your own node module as a javascript file by exporting it as an object on `module.exports.myModule` like this:
```js
module.exports.beBasic = () => "That's so fetch"
```
then in your app just use `const basic = require('./beBasic')`

#### Node Packages
Install node packages in your terminal (inside project folder) with `npm i <pkg-name>` - express, ejs, express-ejs-layouts axios dotenv

#### Add Express to node app
install express using npm
At the top of your code:
```
const express = require('express')
const app = express()
```


Run the app to listen on port 8000:

`app.listen(8000,()=>{console.log('listening on port 8000')})`


Get to home route:
```js
app.get('/',(req,res)=>{
  res.send('hello world route')
})
```
Any other route you can add after the '/' like `/about` or `/blog`. You can use parameters to access a number of values using ":" for example:
`app.get('/animal/:name')` will match for any route with `/animal/SOMETHING` and you can access `name` via the object `req.params.name` in the callback. Adding the asterisk to a route will match anything after it.

Query strings can also be passing in a path after the question mark. These appear in `req.query` under key/value pairs
`https://www.domain.com/some/data?key1=value&key2=value2`

#### Views:
Instead of sending plain text, you can also `res.sendFile(__dirname+'/mydirectory/file.html')` to serve HTML, where `__dirname` is a node variable on the absolute directory.

#### ejs templates
EJS templating engine can be used by setting your files from html to `.ejs`. You need to run ejs middleware like this:
```js
// tell express that we'll use ejs as the view engine
app.set('view engine','ejs')
```
Then you need to change your `res.sendFile` to `res.render` to tell express to use the ejs. The files need to all be in a views directory!
Using ejs you can send variables (as objects) via the optional second parameter to `res.render` as key/value pairs. then you can use these variables in the ejs templates to do some JS. The <%= is when you want to return/show/render a value and the <% is just when you want to do some logic.
```html
<!DOCTYPE html>
<html>
  <head>
    <title>Home Page</title>
  </head>
  <body>
    <h1>Hello, <%= name %>!</h1>
    <% let dogAge = age*7 %>
    <h2>You are <%= dogAge %> in dog years.</h2>
  </body>
</html>
```
With ejs you can also use partials which are part of a page - e.g. a header or footer. This is a separate .ejs file holding header/footer/etc html and you can inject it into any page using:
` <%- include('../partials/header.ejs') %>`


#### layouts
There is an npm package express-ejs-layouts which allows you to use page layouts. It's kind of the opposite of partials where you inject the page content into a predefined layout. You need to add `const ejsLayouts = require('express-ejs-layouts')` and tell express to `app.use(ejsLayouts )` which is a middleware. You need to have a file in your views directory called `layout.ejs` and it needs to look like this:
``` html
<!DOCTYPE html>
<html>
<head>
  <title>Love It or Leave It</title>
</head>
<body>
  <%- body %>
</body>
</html>
```
Then the individual .ejs files you make will be injected into the layout.ejs file where the body part is.

#### Environment Variables:
You can keep certain things 'secret' from your app by putting them in a `.env` file in your root directory of the project. To use them, you need to install the node package `dotenv` and require/config it with the code: `require('dotenv').config()`. Then all variables defined inside your `.env` file can be accessed via `process.env.PORT` (for the key PORT=3000 this would resolve to 3000)
```
PORT=3000
API_KEY=abc123
```

#### gitignore
This file `.gitignore` will tell github which directories or files not to track. Important to include both `.env` (for security) and `node_modules` for size.
- touch .gitignore  
 ```
 node_modules
 .env
```

#### Sequelize
##### Setup
`npm install pg sequelize` to install the required node modules. Then you need to set things up with `sequelize init` which will create the config.json, models and migrations directories. You can edit the config.json to change the db names and remove/edit the username/passwords. Also change the dialect to postgres.
##### Database Creation
To create the database we can type `sequelize db:create db_name` where db_name is the database we want to create.
##### Models
Then create the models by running:
`sequelize model:create --name user --attributes firstName:string,lastName:string,age:integer,email:string`
with whatever table columns and types you want. The word after `--name` will be the name of your model. Running this command will create the js file in your `/models` directory which you should check.
##### Run migration
To migrate the model into your database you can run: `sequelize db:migrate` which will create the table `users` in this case.
##### Include in your app
To include in your app add the code `const db = require('./models')` to whereever your routes are
