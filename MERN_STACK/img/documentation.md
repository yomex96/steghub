## WEB STACK IMPLEMENTATION (MERN STACK) IN AWS

### INTRODUCTION:

### What is the MERN stack?
The 
***MERN stack***
 is a web development framework made up of the stack of MongoDB, Express, React.js, and Node.js. It is one of the several variants of the 
MEAN stack.

***MERN***  Stands For:

- M- MongoDB
- E- ExpressJS
- R-ReactJS
- N-Node.js

When you use the MERN stack, you work with React to implement the presentation layer, 
Express and Node.js to make up the middle or application layer, and MongoDB to create the database layer.

In this MERN stack tutorial, we will utilize these four technologies to develop a basic **To-Do  application** that is able to create a to-do list.

### Setting up the project

MERN lets us create full-stack solutions. So, to leverage its full potential, we will be creating a MERN stack project. For this project, we will create both a back end and a front end. The front end will be implemented with React and the back end will be implemented with MongoDB, Node.js, and Express. We will call the front end client and the back end server.

## Step 0: Prerequisites

__1.__ EC2 Instance of t3.small type and Ubuntu 24.04 LTS (HVM) was lunched in the region of my choosing using the AWS console.
![Lunch Instance](/img/mern_stack_Launch.png)
![Lunchdetails](/img/mernstackdetails.png)


__2.__ Created SSH key pair named __mernstack__ to access the instance on port 22.

![keypair](/img/keypair%208.04.44%20AM.png)

__3.__ The security group was configured with the following inbound rules:

- Allow traffic on port 80 (HTTP) with source from anywhere on the internet.
- Allow traffic on port 443 (HTTPS) with source from anywhere on the internet.
- Allow traffic on port 22 (SSH) with source from any IP address. This is opened by default.

![Security Rules](/img/securitygroup.png)

__4.__ The default VPC and Subnet was used for the networking configuration.

![EC2 Network](/img/mern_network.png)

__5.__ Launched Git Bash and ran the following command:
```
ssh -i mernstack.pem ubuntu@44.211.247.36

```
__6.__ Permission Denied.The warning about the permissions of the private key file ****(Lemp-Server.pem)**** indicates that its permissions are too open. SSH requires strict permissions for private key files for security reasons. 
![Permission Denied](/img/permission_denied.png)

_7.__ To fix this, you need to change the permissions of the private key file. Run the following command in your terminal:

```
chmod 600 mernstack.pem
```
![Permission change](/img/permission_changed.png)

__8.__ After applying these fixes, try SSH-ing into the server again.

```
ssh -i mernstack.pem ubuntu@44.211.247.36
```
![Launch Git Bash](/img/ssh.png)

## Step 1 - BackEnd Configuration
__1.__ __Update ubuntu__
```
sudo apt update
```
![update ubuntu](/img/update_ubuntu.png)

__2.__ __Upgrade ubuntu__
```
sudo apt upgrade
```
![upgrade ubuntu](/img/ubuntu_upgrade.png)

__3.__ __location of Node.js__
```
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
```
![location_](/img/location_nodejs.png)

__4.__ Install Node.js on the server
```
sudo apt-get install -y nodejs
```
![install_nodejs](/img/install_nodejs.png)

__5.__ Verify the node installation with the command 
```
node -v
or 
npm -v
```
![verify_node](/img/nodejs_verification.png)

### Application Code Setup

__6.__ Create a new directory for your To-Do project
```
$ mkdir Todo
```
![directory_todo](/img/directory_todo.png)

__7.__ Verify the **Todo** directory is created with ***ls*** commond
```
$ ls
```
![directory_confirmed](/img/directory_confirm.png)
**Tip**: In order to see some more useful information about files and directories, you can use following combination of keys *ls-lih-*. It will show you different properties and size in human readable format. You can learn more anout different useful keys for ls comman with *ls--help*

__8.__ Change your current directory to the newly created one
```
$ cd Todo
```
![create_todo](/img/cd_todo.png)

__9.__ Initialise the project 

```
npm init
```
___NB: A new file package.json will be created. Follow the prompts after running the command___


## Step 2: Install ExpressJS

__1.__ To use express, install it using npm
```
$ npm install express
```
![install_express](/img/express_install.png)

__2.__ Create a file ***index.js*** with the command bellow
```
 $ touch index.js
```
![index.js created](/img/index.js_created.png)

__3.__ Run ls to confirm that your index.js file is sucessfully created

![index.js confirmation](/img/index.js_confirmation.png)

__4.__ Install the dotenv module
```
$ npm install dotenv
```
![dotenv module](/img/dotenv%20module.png)

__5.__ Open the index.js file with the command bellow
```
$ vim index.js
```
Type the code below into it and save.
```
const express = require('express');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

app.use((req, res, next) => {
  res.header("Access-Control-Allow-Origin", "\*");
  res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
  next();
});

app.use((req, res, next) => {
  res.send('Welcome to Express');
});

app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
```
__NB: use `:w` to save in vim and use `:qa` to exit_

![const_express](/img/const_express.png)

__5.__  Start the server to see if it works. 

```
$ node index.js
```
![port5000](/img/port%205000.png)
If everything goes well, you should see`server running on port 5000` in your terminal

_NB: Ensure you have opened the port `5000` in your security group_

![securityG](/img/edit_securityG.png)

__6.__ Open up your browser and try to access your server's Public IP followed by port `5000`

```
http://34.205.144.109:5000
```

![express_webpage](/img/express_webpage.png)

## Step 2 - Creating the Routes

There are three actions that our To-Do application needs to be able to do:

- Create a task
- Display list of all tasks
- Delete a completed task

Each task will be associated with some particular endpoint and will use differnet standard HTTP request menthods: POST, GET, DELETE.

For each task, I created `routes` that will define various endpoints that the `To-do` app will depend on. 

__1.__ Create a folder `routes`.
```
$ mkdir routes 
```

__2.__ Change directory to `routes` folder.
```
$ cd routes 
```
![directory_route](/img/routes_directories.png)

__3.__ Create a file `api.js` and open the file then write the code below.
```
$ touch api.js
```
__4.__ Open the file with the comand below
```
$ vim api.js
```
__5.__ Copy below code in the file
```
const express = require('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {
 
});

router.delete('/todos/:id', (req, res, next) => {
 
})

module.exports = router;
```
![POST_GET](/img/routers.png)
<br> </br>

## Step 3 - Define the Models

This app makes use of MongoDB, we need to create a model and a schema. <br>A Model is at the heart of JavaScript based applicationa, and it's what makes it interactive. It is also use to defined database schema. <br> The schema is a blueprint of how the database will be constructed. 

To create a schema and a model, install Mongoose which is a Node package that makes working with MongoDB easier.

__1.__ Install `mongoose`

```
npm install mongoose
```
![install_mongoose](/img/install_mongoose.png)

__2.__ Create a new folder in your root directory and name it `models`. Inside it create a file and name it `todo.js` with the following code in it:

```
$ mkdir models && cd models && touch todo.js
```

__3.__ Open the file created with `vim todo.js` then paste the code below in the file

Now, we need to update our routes in api.js to make use of the new model.
```
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

// Create schema for todo
const TodoSchema = new Schema({
  action: {
    type: String,
    required: [true, 'The todo text field is required']
 }
})

// Create model for todo
const Todo = mongoose.model('todo', TodoSchema);

module.exports = Todo;
```
![vim_tod.js](/img/vim_todo_js.png)

__4.__ Now, we need to update our routes in api.js to make use of the new model.

In the Routes directory, Open `api.js`

```
vim api.js
```

Paste this in `routes/api.js`:

```
conconst express = require('express');
const router = express.Router();
const Todo = require('../models/todo');

router.get('/todos', (req, res, next) => {


  // This will return all the data, exposing only the id and action field to the client
  Todo.find({}, 'action')
    .the(data => res.json(data))
    .catch(next);
});

router.post('/todos', (req, res, next) => {
  if (req.body.action) {
    Todo.create(req.body)
      .then(data => res.json(data))
      .catch(next);
  }else {
    res.json({
      error: "The input field is empty"
    })
  }
});

router.delete('/todos/:id', (req, res, next) => {
Todo.findOneAndDelete({"_id": req.params.id})
.then(data => res.json(data))
.catch(next)
})
module.exports = router;
                    
```
![vim_api.js_edit](/img/api.js_edit.png)

## Step 4 - Connecting to a Database

__1.__ [Sign Up](https://www.mongodb.com/products/try-free/platform/atlas-signup-from-mlab)  to mongoDB
   
  ![vim_api_edit](/img/mongoDB%20home%20page.png)

__2.__  Create a cluster, select AWS as the cloud provider and choose a region near you. 

![vim_api_edit](/img/mongoDB.png)

__3.__ Then your Cluster is created.

![mongo_](/img/mongo_cluster.png)

__4.__  Create a user and give it admin access. Click on `database Access` on the left sidebar.

![mongos_admin](/img/admin_mongoDB.png)

__5.__ Click on `Browse collection`, to create a database.

![mongoDB_DB](/img/mongoDB_Database.png)

__5.__ Edit your IP Access List Entry to anywhere to access your database

![mongoDB_DB](/img/mongoose_nextworkaccess.png)

__6.__ Click `Connect` to connect your Database with a Driver. Select `mongoose` and copy the `Connection strings` 
  <br></br>
![mongoose_connect](/img/connect_mern-stack_mongoose.png)



__7.__ Create a file in the `Todo` directory and name it `.env`

```
$ touch .env

```
![mongos_admin](/img/env_directory.png)

__7.__ Open the file with vim editor

```bash
vi .env

# Paste your database connection string in it 

DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'
```
Ensure to update `<username>` `<password>` `<network-address>` and  `<database>` according to your setup.

![mongos_admin](/img/connection_string.png)

__8.__ Update `index.js` to reflect the use of `.env` so that Node.js can connect to the DB. 

```bash
vim index.js
```

Paste this:
```bash

const mongoose = require('mongoose');
const routes = require('./routes/api');
const path = require('path');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

// Connect to the database
mongoose.connect(process.env.DB, { useNewUrlParser: true,useUnifiedTopology: true })
  .then(() => console.log(`Database connected successfully`))
   .catch((err) => console.log(err));

// Since mongoose's Promise is deprecated, we override it with Node's Promise
mongoose.Promise = global.Promise;

app.use((req, res, next) => {
  res.header("Access-Control-Allow-Origin", "\*");
  res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
  next();
});

app.use(bodyParser.json());

app.use('/api', routes);

app.use((err, req, res, next) => {
  console.log(err);
  next();
});

app.listen(port, () => {
  console.log(`Server running on port ${port}`)
});
```
<br></br>
![node](/img/reflect.env.png)

__8.__  Start Nodejs Server

```bash
node index.js
```
![node](/img/server_running.png)

## Step 5 - Test Backend Code without Frontend using RESTful API

So far we have written backend part of out  **To-Do** application, and configured a database, but we do not have a frontend UI yet. We need a ReactJS code to achieve that. 

__1.__  You'll need to install [Postman](https://www.getpostman.com) or you can use VScode Extension called ThunderClient. 

![postman](/img/postman_home.png)

Next, we open send an API request, and create a POST request to our API(make sure your server is running on your terminal)

__2.__  Create a POST method  and navigate to `http://publicIP:5000/api/todos.` and type this in the `body`

```
POST publicIP:5000/api/todos/
```
![postman](/img/post_ipaddress.png)
```
{
  "action": "Database is to be connected"
}
```
![post](/img/postman.png)
```
{
  "action": "Ensuring Database is updated"
}
```
![post](/img/post%202.png)

__3.__ To list all the POST request, we run the GET method

```
GET publicIP:5000/api/todos/
```
```
{
  "action": "Database is confirmed"
}
```
![post](/img/get_postman.png)

__4.__  To Delete a specific POST require, we run this on the DELETE method

```
Delete publicIP:5000/api/todos/<id>
```
![post](/img/delete_postman.png)

## STEP 6 - Create the FrontEnd

it is time to create an interface for the client to interact with the API. To start out with the frontend of the todo app, you will use the `create-react-app` command to scaffold your app.
In the same `todo` directory

__1.__  Run this command

```
npx create-react-app client
```
![Client](/img/client_folder.png)

__2.__ Install concurrently 
Concurrently is used to run more than one command simultaneously from the same terminal window. 

```
npm install concurrently --save-dev
```
![concurrently](/img/concurentlly_install.png)

__3.__ Install nodemon 
Nodemon is used to run the server and monitor it as well. If there is any change in the server code, Nodemon will restart it automatically with the new changes.

```
npm install nodemon --save-dev
```
![nodemon](/img/nodemon_install.png)

__4.__Configure nodemon in the package.json in the `Todo `directory

```
nano package.json
```

Paste this in the Scripts block:
```
{
  // ...
  "scripts": {
    "start": "node index.js",
    "start-watch": "nodemon index.js",
    "dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
  },
  // ...
}
```
![json_package](/img/package.json.png)

__4.__ Configure Proxy in package.json located in the `Client` directory.

```
$ cd client
vi package.json
```

then add

```bash 
{
  
  "proxy": "http://localhost:5000"
}
```
![proxy](/img/proxy_added.png)

***NOTE:*** The whole purpose of adding the proxy configuration in number 3 above is to make it possible to access the application directly from the browser by simply calling the server url like http://localhost:5000 rather than always including the entire path like http://localhost:5000/api/todos.

__5.__ Run `npm run dev` and make sure you are in the todo directory and not in the client directory.

```
npm run dev
```


![run_dev](/img/run_dev.png)

_Your app will be open and running on PublicIP:3000_

_Enusre you open port 3000 in your AWS Security group_

![3000_securitygroup](/img/3000_securityG.png)

![scr](/img/src:app.js.png)

## Step 7 — Creating the React Components

For your todo app, there will be two state components and one stateless component.

__1.__ Inside your `src` folder create another folder called  `components` and inside it create three files `Input.js`, `ListTodo.js`, and `Todo.js`.

```
$ cd client && cd src && mkdir components && cd components

$ touch Input.js ListTodo.js Todo.js
```
__2.__ Open Input.js
```bash
vim Input.js
```
```

import React, { Component } from 'react';
import axios from 'axios';

class Input extends Component {
  state = {
    action: " "
  }

  addTodo = () => {
    const task = { action: this.state.action }

    if (task.action && task.action.length > 0) {
      axios
        .post('/api/todos', task)
        .then((res) => {
          if (res.data) {
            this.props.getTodos();
            this.setState({ action: "" })
          }
        })
        .catch((err) => console.log(err));
    } else {
      console.log('input field required');
    }
  }

  handleChange = (e) => {
    this.setState({
      action: e.target.value,
    })
  }

  render() {
    let { action } = this.state;
    return (
      <div>
        <input type="text" onChange={this.handleChange} value={action} />
        <button onClick={this.addTodo}>add todo</button>
      </div>
    )
  }
}

export default Input
```
![js_edit](/img/js_edit.png)

_To make use of axios, which is a Promise-based HTTP client for the browser and Node.js, you will need to navigate to your client directory from your terminal:_

```
$ cd client
```

Then run 

```
npm install axios
```
![js_edit](/img/install_axios.png)

__3.__ Go back to the component directory

```bash 
$ cd src/components
```

__4.__ Open `ListTodo.js` with vim text editor 

```
nano ListTodo.js
```
```
import React, { Component } from 'react';
import axios from 'axios';

import Input from './Input';
import ListTodo from './ListTodo';

class Todo extends Component {
  state = {
    todos: []
  }

  componentDidMount() {
    this.getTodos();
  }

  getTodos = () => {
    axios.get('/api/todos')
      .then(res => {
        if (res.data) {
          this.setState({
            todos: res.data
          });
        }
      })
      .catch(err => console.log(err));
  }

  deleteTodo = (id) => {
    axios.delete(`/api/todos/${id}`)
      .then(res => {
        if (res.data) {
          this.getTodos();
        }
      })
      .catch(err => console.log(err));
  }

  render() {
    let { todos } = this.state;
    return (
      <div>
        <h1>My Todo(s)</h1>
        <Input getTodos={this.getTodos} />
        <ListTodo todos={todos} deleteTodo={this.deleteTodo} />
      </div>
    );
  }
}
export default Todo;
```

![img](/img/todo.js_edit.png
)

__5.__ Adjust the React code by Deleting the logo and adjust `App.js` to look like this:

```
$ cd ..
```

Open App.js with nano text editor

```
vim App.js
```
Paste this:

```bash

import React from 'react';
import Todo from './components/Todo';
import './App.css';

const App = () => {
  return (
    <div className="App">
      <Todo />
    </div>
  );
};

export default App;
```
![](/img/edit_app.js.png)

__6.__ In the src directory, open the  `App.css`

```
vim App.css
```
Paste the following code into it
```
.App {
  text-align: center;
  font-size: calc(10px + 2vmin);
  width: 60%;
  margin-left: auto;
  margin-right: auto;
}

input {
  height: 40px;
  width: 50%;
  border: none;
  border-bottom: 2px #101113 solid;
  background: none;
  font-size: 1.5rem;
  color: #787a80;
}

input:focus {
  outline: none;
}

button {
  width: 25%;
  height: 45px;
  border: none;
  margin-left: 10px;
  font-size: 25px;
  background: #101113;
  border-radius: 5px;
  color: #787a80;
  cursor: pointer;
}

button:focus {
  outline: none;
}

ul {
  list-style: none;
  text-align: left;
  padding: 15px;
  background: #171a1f;
  border-radius: 5px;
}

li {
  padding: 15px;
  font-size: 1.5rem;
  margin-bottom: 15px;
  background: #282c34;
  border-radius: 5px;
  overflow-wrap: break-word;
  cursor: pointer;
}

@media only screen and (min-width: 300px) {
  .App {
    width: 80%;
  }

  input {
    width: 100%;
  }

  button {
    width: 100%;
    margin-top: 15px;
    margin-left: 0;
  }
}

@media only screen and (min-width: 640px) {
  .App {
    width: 60%;
  }

  input {
    width: 50%;
  }

  button {
    width: 30%;
    margin-left: 10px;
    margin-top: 0;
  }
}
```
![img](/img/App.css_edit.png)

__7.__ In the src directory, open the  `index.css`

```
vim index.css
```

Paste this:

```
body {
  margin: 0;
  padding: 0;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen", "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  box-sizing: border-box;
  background-color: #282c34;
  color: #787a80;
}

code {
  font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New", monospace;
}
```
![index.css](/img/index.css.png)

__8.__  Go back to the `Todo` directory 

```
$ cd  ../..
```

__9.__  On the Todo directory run

```bash
npm run dev
```
![](/img/run_dev_server.png)

__10.__ React build would compile sucessfully and both backend and front end will run concurrently through the ports we earlier opened

Open in the browser http//publicIP:3000

![todolist](/img/my%20_todo.png)

**Note** To make sure it is working, let us type a word on the insert then press add todo, it should reflect on the list


![todolist](/img/todolist_test.png)

## You have just created a simple To-Do and deployed it to MERN Stack.You wrote a frotend application using React.js that communicated with a backened application writte using Expressjs.You also created a Mongodb backend for storing tasks in a database.
