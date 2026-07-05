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

Run the server with `nodeman server.js`


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

