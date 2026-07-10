# men stack notes

| CRUD Action | RESTful Action | HTTP Method | Definition |
| ----------- | -------------- | ----------- | ------------------------ |
|  | `new` | `GET` | Show a form to create a new item |
| Create | `create` | `POST` | Add a new item to the database |
| Read | `index` | `GET` | Show all items |
| Read | `show` | `GET` | Show one specific item |
| | `edit` | `GET` | Show a form to edit an existing item |
| Update | `update` | `PUT` | Save changes to an existing item |
| Delete | `delete` | `DELETE` | Remove an item from the database |

## SET UP 

- create a directory
- create server file `touch server.js`
- create `.gitignore` file
- initialise a node project with `npm init -y`
- install express `npm i express`
- install express and morgan `npm i express morgan`

### Add `node_modules` to `.gitignore`

.gitignore
```bash
node_modules
```
### Write Server Boilerplate

server.js
```js
// bring express into our server
const express = require('express')
const morgan = require('morgan')

//actually use express 
const app = express()
app.use(morgan('dev'))

app.listen(3000, function(){
    console.log('listening to port 3000')
})
```

Run the server with `nodemon server.js`


<img width="467" height="126" alt="Screenshot 2026-07-05 at 11 23 14 AM" src="https://github.com/user-attachments/assets/f1badb10-c67c-414d-9046-7b71e14cf424" />

navigate to `http://localhost:3000/` to view our server.

use `ctrl + c` to stop the server in the terminal.

### Creating a Test Route 

```js
app.get('/test', function(req, res){
    res.send('<h1>This is a test</h1>')
})
```
<img width="341" height="126" alt="Screenshot 2026-07-05 at 12 30 47 PM" src="https://github.com/user-attachments/assets/eebef3a2-cc12-4369-bd3a-692e674dad8a" />

navigate to http://localhost:3000/test

### Using Request Parameters

```js
app.get('/:userId', function(req, res){
    res.send(`User ID: ${req.params.userId}`)
    console.log(req.params.userId)
})
```
Navigate to http://localhost:3000/2490

<img width="341" height="126" alt="Screenshot 2026-07-05 at 2 11 50 PM" src="https://github.com/user-attachments/assets/e664f348-c2bf-41b9-8d7c-79c6812e4682" />

## Rendering EJS

- install ejs `npm i ejs`
- create a view folder `mkdir views`
- create an ejs file `touch views/home.ejs`
- add html boilderplate with `!`

  ### home.ejs
  
 
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Home</title>
</head>
<body>
  <h1>We are rendering an EJS page!</h1>
</body>
</html>
```


  - render `ejs` page using a controller like this one:
 
  ``` js
  app.get('/', function(req, res){
    res.render('home.ejs')
})
  ```
<img width="713" height="267" alt="Screenshot 2026-07-06 at 12 40 11 PM" src="https://github.com/user-attachments/assets/c48d7f9a-f78c-4911-8f00-1e1811477a01" />

### EJS Syntax
- to use javascript in an ejs file, I need a scriplet tag:
```ejs
<% let user = 'Jo' %>
```
- to display javascript values from an ejs file, I need an output tag:
```ejs
<%= user %>
```

### Pass data from the controller 
- Use the locals objects inside the render method:
```js
res.render('home.ejs', {
    title : 'Home Page',
    )}
```

Now i can use the `title` varibale in my `home.ejs` file.
home.ejs 
```js
<h1><%=title%></h1>
```

### Using For Each in `ejs`

```js
<ul>
        <% inventory.forEach(function(item){ %>
          <li> <%= item.name %>: <%= item.qty %> </li>
        <% }) %> 
    </ul>
```
### Pass data from the controller
Use the locals object inside the render method:
```ejs
res.render('home.ejs', {
    title: 'Home Page',
})
```
Now I can use the title variable in my home.ejs file.

home.ejs
```ejs
<h1><%= title %></h1>
Using forEach in ejs
<ul>
    <% inventory.forEach(function(item){ %>
    <li><%= item.name %></li>
    <% }) %>
</ul>
```
<img width="581" height="278" alt="Screenshot 2026-07-07 at 10 01 56 AM" src="https://github.com/user-attachments/assets/40c93ee8-4c1d-402a-86c6-9f1b13326639" />

### Create Dynamic links to a `show` page
`item.name` is dynamically showing up, The link is also dynamically changing with the item. (see `forEach` above).
```ejs
<a href="/<%= item.id %>"> <%= item.name %> </a>
```
We should see the URL change in the browser. 

### Reusable Nav with partials
Create a folder called `partials` inside of `views` 
Create a file called `nav.ejs` inside of `partials`

Nav.ejs
``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title><%= title %></title>
    <link rel="stylesheet" href="/stylesheets/style.css">
</head>
<body>
    <nav>
        <a href="/">Home</a>
    </nav>

```
Include the `nav` in other files with this statement:
``` ejs
<%- include('./partials/nav') %>
```
### Adding Css & images with `public`

- Create a folder called `public` 
- Create a folder called `stylesheets` inside of `public`
- Create a filde called `style.css` inside of `stylesheets`
- Create a folder called `images` inside of `public` 
- Add the below in server.ejs

Configure out server to look inside the `public` folder for static files:
server.js
``` js
// require the path from node at the top
const path = require('path')

//use static middleware with other middleware like morgan
app.use(express.static(path.join(__dirname, "public")))
```

- Link the stylesheet in the head of our `html` files (inside `nav` partials if we're using partials.

``` ejs
<link rel="stylesheet" href="/stylesheets/style.css">
```

### Images
- Create an `images` folder inside of our `public` folder
- link the images like normal: <img src="/images/pexels-i-slam-abruev-2157269750-37959543.jpg" alt="">

### The repeatable pattern
Most Express apps that use MongoDB will follow this pattern:

- Install mongoose and dotenv.
- Store the connection string in .env.
- Add .env to .gitignore.
- Load dotenv in server.js.
- Import mongoose.
- Connect with mongoose.connect(process.env.MONGODB_URI).

👇🏼👇🏼👇🏼They are below👇🏼👇🏼👇🏼

## link database

### Install mongoose and dotenv from NPM 
`npm i mongoose dotenv`

### create an `.env` file 
- we add it to .gitignore under `node_modules`
- `.env` file is to store values without pushing it to github
For example
```js
SECRET_NUMBER=13
PASSWORD=12345
```
- we put `MONGODB_URI=` in `.env`
- the URL always goes between the `/` and `?` For example `mongodb.net/example_db?appName`

### Inlcude dotenv & mongoose in `server.js`
```js
const dotenv = require('dotenv').config() // making .env file available
const mongoose = require('mongoose')
```
- Connect us to MongoDB using connection string in `.env`
```js
mongoose.connect(process.env.MONGODB_URI)
```
- Make sure mongoose connection is running by adding this to server.js
`server.js`
```js
mongoose.connection.on("connected", () => {
  console.log(`Connected to MongoDB ${mongoose.connection.name} 🥭`);
});
```

### How to build a model 

- we have to add mongoose at the top of our `exmaple.js` folder
```js
const mongoose = require('mongoose')
```
- creating it
example:
```js
const fruitSchema = new mongoose.Schema({
    name: String,
    isReadyToEat: Boolean, 
})


// create a model to be used in other parts of our
//application
const Fruit = mongoose.model('Fruit', fruitSchema)
```
- its okay for the model to have an upper Case
- final part is to export
```js
module.exports = Fruit
```

### Include the model in server.js

```js
// Import the Fruit model
const Fruit = require("./models/fruit.js");
```

- we add the content
still in `server.js`

```js
//this route will change often
app.get("/fruits", async (req, res) => {
  // ❓ create a fruit object
  const fruitData = {}
  fruitData.name = 'Strawberry'
  fruitData.isReadyToEat = false

  // ❓ use a mongoose method to add it to the database 

  //we created a var to send the new fruit 
  let createdFruit = await Fruit.create(fruitData)

  // we pass the object in the () 👆🏼
//   its using the creating method to send to the database

  // view the created fruit
  res.send(fruitData)

});

```
### To view Data in MonoDB site

- we check mongoDB
- Projects Overview
- Browse collection
- clusters
<img width="581" height="505" alt="Screenshot 2026-07-09 at 12 35 40 PM" src="https://github.com/user-attachments/assets/ef95af5b-93cc-430a-bfb0-08978d69ed0f" />
- open fruit_db
- we can access data
<img width="1411" height="505" alt="Screenshot 2026-07-09 at 12 36 22 PM" src="https://github.com/user-attachments/assets/d3976601-e030-4570-a3f3-e832775d67a4" />

### How to find all fruits 
```js
//this route will change often
app.get("/fruits", async (req, res) => {

    //use mangoose to find all the fruits 
  let allFruits = await Fruit.find()


  // view the created fruit
  res.send(allFruits)

});
```

<img width="1411" height="505" alt="Screenshot 2026-07-09 at 12 47 54 PM" src="https://github.com/user-attachments/assets/1f233a3a-e7d1-4b21-9d8c-7dce2b435805" />

### How to find a specific fruit 

```js
//this route will change often
app.get("/fruits", async (req, res) => {

    //use mangoose to find a specific with name fruit
  let allMangos = await Fruit.find({name: 'Mango'})


  // view the created fruit
  res.send(allMangos)

});
```
### To find one 
you just add `One` after `find`
```js
  let allFalses = await Fruit.findOne({isReadyToEat: true})
```

### To find one and update 
you just add `One` and `Update`

```js
 let allFalses = await Fruit.findOneAndUpdate({name: 'Mango'}, {name: "watermelon" },
  {new: false})

```

### To find one and update by id 
you just add `ByIdAndUpdate` after `find`
```js
let updatedFruit = await Fruit.findOneAndUpdate(
  { _id: "6a4f6c58c50aacd3bb29b169" },
  { name: "Green Apple" },
  { new: true }
)

//or

const updatedFruit = await Fruit.findByIdAndUpdate(
    "6a4e80051c3a032571e4ae92",
    { name: "Green Apple" },
    { new: true }
  );
```

### Delete one Fruit 
```js
let deletedFruit = await Fruit.findByIdAndDelete("6a4f6c5b26b63467c4db784b")
```


### Find by Id
```js
let findfruitbyid = await Fruit.findById("6a4e80051c3a032571e4ae92")
```

### We need to add this in server.js to keep track of form or idk for it just to work
```js
//above morgan
app.use(express.urlencoded({ extended: false }));
```

### To see what we entered in the form 

```js
app.get("/fruits/new", async (req, res) => {
  res.render('new.ejs')
});

// POST /fruits (creates fruit in database)
app.post('/fruits', (req, res) => {
res.send(req.body)
})

```
its this like 
```js
res.send(req.body)
```

the form in new.ejs
```ejs
 <h1> Create a New Fruit </h1>
    <form action="/fruits" method="POST">
        <label for="name">Name:</label>
        <input type="text" id="name" name="name">

        <label for="isReadyToEat">Ready To Eat?</label>
        <input type="checkbox" id="isReadyToEat">
        <button type="submit">Add Fruit</button>
    </form>
</body>
```

to show only the name
```js
app.post('/fruits', (req, res) => {
res.send(req.body.name)
})
```

