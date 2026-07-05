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
- initialise a node project with `npm init -y`
- install express `npm i express`

### Write Server Boilerplate

server.js
```js
// bring express into our server
const express = require('express')

//actually use express 
const app = express()

app.listen(3000, function(){
    console.log('listening to port 3000')
})
```
