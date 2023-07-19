# **SIMPLE TO-DO APPLICATION ON MERN WEB STACK**

### In this project, we are tasked to implement a web solution 
# **SIMPLE TO-DO APPLICATION ON MERN WEB STACK** based on MERN stack in AWS Cloud.

## **MERN = MongoDB + ExpressJS + ReactJS + Node.js**
<img width="638" alt="MERN-stack image" src="https://github.com/Bamideleflint/Darey-PBL/assets/122679229/90523fdd-a9d2-40c9-b20f-e624c26bb003">

As shown in the illustration above, a user interacts with the ReactJS UI components at the application front-end residing in the browser. This frontend is served by the application backend residing in a server, through ExpressJS running on top of NodeJS.

Any interaction that causes a data change request is sent to the NodeJS based Express server, which grabs data from the MongoDB database if required, and returns the data to the frontend of the application, which is then presented to the user.

In order to complete this project we will need an AWS account and a virtual server with Ubuntu Server OS.
We log on to AWS Cloud Services and create an EC2 Ubuntu Instance. When creating an instance, choose keypair authentication downloaded to local system.
![Aws EC2 Instance](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/8212f37d-3a1c-4b47-b5da-6448120d78de)

STEP 1 – BACKEND CONFIGURATION

* Update ubuntu  `sudo apt update`
* Upgrade ubuntu `sudo apt upgrade`
![Sudo Upgrade Restart](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/53a26e35-8909-4795-bdc9-ff6eddd0681f)

*  Get location of Node.js software from Ubuntu repositories. `curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -`
*  Install Node.js on the server `sudo apt-get install -y nodejs`
*  Note: The command above installs both nodejs and npm. NPM is a package manager for Node like apt for Ubuntu, it is used to install Node modules & packages and to manage dependency conflicts.
* Verify the node installation `node -v`
* Verify the npm installation `npm -v`
* ![NodeJS version installation](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/1cb6ad68-31ed-48c2-ab78-ba2a84840fbc)
* Create a new directory for our To-Do project `mkdir Todo`
* We can verify the directory was created with the command `ls`
* ![Todo Directory Creation](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/a98b8cfe-82d8-44e5-ad35-242c87b72bde)
* Changing our current directory to the newly created one `cd Todo`
*  Initialise our project, so that a new file named package.json will be created. This file will normally contain information about our application and the dependencies that it needs to run. Follow the prompts after running the command. Press Enter several times to accept default values, then accept to write out the package.json file by typing yes. `npm init`
*  Run `ls` to confirm package.json file was created.                                                                                                             ![packagejson file created](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/d40ab80b-f6b7-4aca-870f-841bf4ef0e14)

*  **Express is a framework for Node.js, therefore a lot of things developers would have programmed is already taken care of out of the box. Therefore it simplifies development, and abstracts a lot of low level details. For example, Express helps to define routes of your application based on HTTP methods and URLs.**
* Install express `npm install express`
![expressjs installed](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/92b20e64-8106-42f4-9bf4-389a902bd79c)
*  Create a file index.js `touch index.js`
*  Run `ls` to confirm that your index.js file is successfully created
 ![indexjs file created](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/d670e2ac-7310-45db-a37c-bdb1f57f0f60)
*  Install the dotenv module `npm install dotenv`
*  Open the index.js file with `vim index.js` to edit and put in the code below
```
{
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
console.log(`Server running on port ${port}`)
});
}
```
* To test if the server is running on port 5000 on our terminal `node index.js`
* ![running on port 5000](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/4b993a5b-4c59-4b54-b5b7-27fa31272c46)
* To be able to test it on our browser, we will go into our EC2 Instance security group and edit the inbound rule to open port 5000.
 ![AWS securityinboundrules edited for Port5000](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/dcb27b82-52dd-4213-9bb5-d75d3b35dfec)
* Accessing the server on our browser with either a Public IP or Public DNS name followed by port 5000 `http://<PublicIP-or-PublicDNS>:5000`. The server's Public IP and public DNS name can be gotten on our AWS web console in the EC2 Instance created or Run ` curl -s http://169.254.169.254/latest/meta-data/public-ipv4` for Public IP address or `curl -s http://169.254.169.254/latest/meta-data/public-hostname` for Public DNS name on the terminal.
* ![ExpressWebPage](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/771ad8d8-dca6-40c0-b1e5-c079e822ceba)

* **There are three actions that our To-Do application needs to be able to do**
1. Create a new task
2. Display list of all tasks
3. Delete a completed task
Each task will be associated with some particular endpoint and will use different standard HTTP request methods: POST, GET, DELETE.

For each task, we need to create routes that will define various endpoints that the To-do app will depend on.
* Create folder routes `mkdir routes`
* Change directory to routes folder `cd routes`
* Create a file api.js `touch api.js`
*  ![apijs file created](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/af22dd0a-1543-4ed2-ab09-232545d2a050)
* Open file api.js with `vim api.js` and populate with code below
```
{
const express = require ('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {

});

router.delete('/todos/:id', (req, res, next) => {

})

module.exports = router;
}
```
* The app is going to make use of Mongodb which is a NoSQL database, we need to create a model. A model is at the heart of JavaScript based applications, and it is what makes it interactive. We will also use models to define the database schema . This is important so that we will be able to define the fields stored in each Mongodb document. The Schema is a blueprint of how the database will be constructed, including other data fields that may not be required to be stored in the database. These are known as virtual properties
* To create a Schema and a model, install mongoose which is a Node.js package that makes working with mongodb easier.
* Change directory back Todo folder with `cd ..` and install Mongoose `npm install mongoose`
 ![mongoose install](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/0281d372-5e00-4e78-a79c-32ca18cf1fd3)
* Create a new folder called models `mkdir models`
* Change directory into the newly created ‘models’ `cd models`
* Inside the models folder, create a file and name it todo.js `touch todo.js`
* ![todojs for modelsfolder](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/7f357dab-1f95-469c-9933-44d27519d2da)
* Open the created file todo.js with `vim todo.js` and paste the code below
```
{
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

//create schema for todo
const TodoSchema = new Schema({
action: {
type: String,
required: [true, 'The todo text field is required']
}
})

//create model for todo
const Todo = mongoose.model('todo', TodoSchema);

module.exports = Todo;
}
```
* we need to update our routes from the file api.js in ‘routes’ directory to make use of the new model.
* In Routes directory, open api.js with `vim api.js`, delete the code inside with `:%d` command and paste the code below into it then save and exit.
```
{
const express = require ('express');
const router = express.Router();
const Todo = require('../models/todo');

router.get('/todos', (req, res, next) => {

//this will return all the data, exposing only the id and action field to the client
Todo.find({}, 'action')
.then(data => res.json(data))
.catch(next)
});

router.post('/todos', (req, res, next) => {
if(req.body.action){
Todo.create(req.body)
.then(data => res.json(data))
.catch(next)
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
}
```

### **MONGODB DATABASE**
*We need a database where we will store our data. For this we will make use of mLab. mLab provides MongoDB database as a service solution (DBaaS), so to make life easy, we will need to sign up for a shared clusters free account, which is ideal for our use case. Read about how to sign up. Note: select **AWS** as the cloud provider, and choose a region near you. Allow access to the MongoDB database from anywhere (Not secure, but it is ideal for testing). Under the **NETWORK ACCESS**, when adding **IP ACCESS List Entry** click **ALLOW ACCESS FROM ANYWHERE** and change the time of deleting the entry from 6 Hours to 1 Week, write down the informations used while creating cluster*

* In the index.js file, we specified process.env to access environment variables, but we have not yet created this file. So we need to do that in our Todo directory and name it .env with code `touch .env`
* Edit the file `vi .env`
* When signing up for **MongoDB** and creating our first cluster, we would have created a username, password, database name etc. which would be needed while editing our .env file.
* Add the connection string to access the database in it, just as below:
* DB = 'mongodb+srv://*username*:*password*@*network-address*/*dbname*?retryWrites=true&w=majority'
* Ensure to update *username*, *password*, *network-address* and *database* according to our setup

***How to get our connection string***
1. Click Database inside MongoDB
2. Click CONNECT inside the cluster we created
3. Click MongoDB Drivers under Connect to your application
4. Change the DRIVER to Node.js before copying the connection string
	*Note: you can copy the string first to a NotePad for easy edit before pasting inside .env file*

* We need to update the index.js to reflect the use of .env so that Node.js can connect to the database.
* Steps to delete the previous content in our index.js before pasting the new content.
1. Open the file with `vi index.js`
2. Press esc
3. Type :
4. Type %d
5. Hit Enter
The entire content will be deleted, then,
* Press i to enter the insert mode in vim, then copy and paste the code below
```
{
const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
const routes = require('./routes/api');
const path = require('path');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

//connect to the database
mongoose.connect(process.env.DB, { useNewUrlParser: true, useUnifiedTopology: true })
.then(() => console.log(`Database connected successfully`))
.catch(err => console.log(err));

//since mongoose promise is depreciated, we overide it with node's promise
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
}
```
* *Using environment variables to store information is considered more secure and best practice to separate configuration and secret data from the application, instead of writing connection strings directly inside the index.js application file.*
* Start server using the command: `node index.js`, we will get a message **‘Database connected successfully’**, if so – we have our backend configured. Now we are going to test it.
* ![Indexjs server running](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/81859ee0-9f24-4c48-8142-677196f47bf5)

*  **Testing Backend Code without Frontend using RESTful API**
* *So far we have written backend part of our To-Do application, and configured a database, but we do not have a frontend UI yet. We need ReactJS code to achieve that. But during development, we will need a way to test our code using RESTfull API. Therefore, we will need to make use of some API development client to test our code.*
* We will install and use Postman to test our API.
* We will test all the API endpoints and make sure they are working. For the endpoints that require body, we should send JSON back with the necessary fields since it’s what we setup in our code.

* Open Postman, create a POST request to the API http://PublicIP-or-PublicDNS:5000/api/todos. This request sends a new task to our To-Do list so the application could store it in the database.
* ![PostRequestImage](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/5f2bb6f0-e2db-4bb4-b4c0-6957dad8170e)
* Note: Set header key *Content-Type* as *application/json*
* ![CreatePostRequestInPostman](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/24534e6e-dee1-4d77-a352-5358b9bda38a)
* Create a GET request to your API on http://PublicIP-or-PublicDNS:5000/api/todos. This request retrieves all existing records from our To-do application (backend requests these records from the database and sends it us back as a response to GET request).
* ![GetRequestBody Postman](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/0d9399d6-b229-465f-a915-c19492eb135c)

* Create a DELETE request to your API on http://PublicIP-or-PublicDNS:5000/api/todos/ID. This request deletes records from our To-do application.
* ![DeleteRequestIn Postman](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/0390ce5b-0a13-44c6-8910-62a91a20203b)
* **We have successfully created our Backend, now let go create the Frontend.**

## **STEP 2 – FRONTEND CREATION**

* Since we are done with the functionality we want from our backend and API, it is time to create a user interface for a Web client (browser) to interact with the application via API. To start out with the frontend of the To-do app, we will use the create-react-app command to scaffold our app.
* In the same root directory as our backend code, which is the Todo directory, run: `npx create-react-app client` to create a new folder in our Todo directory called client, where we will add all the react code.
* Before testing the react app, there are some dependencies that need to be installed.
  
	1. Install concurrently. It is used to run more than one command simultaneously from the same terminal window: `npm install concurrently --save-dev`
	2. Install nodemon. It is used to run and monitor the server. If there is any change in the server code, nodemon will restart it automatically and load the new changes: `npm install nodemon --save-dev`
 3. In Todo folder open the package.json file. Change the highlighted part of the below screenshot and replace with the code below.
    ![Script Highlight](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/5ba5d3ae-b06f-4bac-9c26-36ea6110e865)
```
{
"scripts": {
"start": "node index.js",
"start-watch": "nodemon index.js",
"dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
},
}
```
* **Configure Proxy in package.json**
  1. Change directory to ‘client’ `cd client`
  2. Open the package.json file `vi package.json`
  3. Add the key value pair in the package.json file "proxy": "http://localhost:5000".
The whole purpose of adding the proxy configuration in number 3 above is to make it possible to access the application directly from the browser by simply calling the server url like http://localhost:5000 rather than always including the entire path like http://localhost:5000/api/todos

* Now, ensure you are inside the Todo directory, and simply do: `npm run dev`
* Our app should open and start running on localhost:3000
* ![LocalHost3000React](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/aeb50082-e9cb-473b-8192-2405198f0c75)
*Important note: In order to be able to access the application from the Internet we have to open TCP port 3000 on EC2 by adding a new Security Group rule. *
* *Creating our React Components
One of the advantages of react is that it makes use of components, which are reusable and also makes code modular. For our Todo app, there will be two stateful components and one stateless component.*
* From your Todo directory run: `cd client`
* Move to the src directory: `cd src`
* Inside our src folder create another folder called components: `mkdir components`
* Move into the components directory with: `cd components`
* Inside ‘components’ directory create three files Input.js, ListTodo.js and Todo.js. `touch Input.js ListTodo.js Todo.js`
* Open Input.js file: `vi Input.js`
* Copy and paste the code below:
```
{
import React, { Component } from 'react';
import axios from 'axios';

class Input extends Component {

state = {
action: ""
}

addTodo = () => {
const task = {action: this.state.action}

    if(task.action && task.action.length > 0){
      axios.post('/api/todos', task)
        .then(res => {
          if(res.data){
            this.props.getTodos();
            this.setState({action: ""})
          }
        })
        .catch(err => console.log(err))
    }else {
      console.log('input field required')
    }

}

handleChange = (e) => {
this.setState({
action: e.target.value
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
}
```
* To make use of Axios, which is a Promise based HTTP client for the browser and node.js, we need to cd into our client from our terminal and run yarn add axios or npm install axios.
* Move to the src folder: `cd ..`
* Move to clients folder: `cd ..`
* Install Axios: `npm install axios`
* Go to ‘components’ directory: `cd src/components`
* After that open ListTodo.js: `vi ListTodo.js`
* In the ListTodo.js copy and paste the code below.
```
{
import React from 'react';

const ListTodo = ({ todos, deleteTodo }) => {

return (
<ul>
{
todos &&
todos.length > 0 ?
(
todos.map(todo => {
return (
<li key={todo._id} onClick={() => deleteTodo(todo._id)}>{todo.action}</li>
)
})
)
:
(
<li>No todo(s) left</li>
)
}
</ul>
)
}

export default ListTodo
}
```
* Then in our Todo.js file write the following code below.
```
{
import React, {Component} from 'react';
import axios from 'axios';

import Input from './Input';
import ListTodo from './ListTodo';

class Todo extends Component {

state = {
todos: []
}

componentDidMount(){
this.getTodos();
}

getTodos = () => {
axios.get('/api/todos')
.then(res => {
if(res.data){
this.setState({
todos: res.data
})
}
})
.catch(err => console.log(err))
}

deleteTodo = (id) => {

    axios.delete(`/api/todos/${id}`)
      .then(res => {
        if(res.data){
          this.getTodos()
        }
      })
      .catch(err => console.log(err))

}

render() {
let { todos } = this.state;

    return(
      <div>
        <h1>My Todo(s)</h1>
        <Input getTodos={this.getTodos}/>
        <ListTodo todos={todos} deleteTodo={this.deleteTodo}/>
      </div>
    )

}
}

export default Todo;
}
```
* We need to make little adjustment to our react code. Delete the logo and adjust our App.js 
* Move to the src folder: `cd ..`
* Edit  App.js `vi App.js` inside src folder and paste the code below into it.
```
{
import React from 'react';

import Todo from './components/Todo';
import './App.css';

const App = () => {
return (
<div className="App">
<Todo />
</div>
);
}

export default App;
}
```
* In the src directory open the App.css with: `vi App.css` and paste code below:
```
{
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
width: 100%
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
}
```
* In the src directory open the index.css with: `vi index.css` and paste the code below:
```
{
body {
margin: 0;
padding: 0;
font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen",
"Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue",
sans-serif;
-webkit-font-smoothing: antialiased;
-moz-osx-font-smoothing: grayscale;
box-sizing: border-box;
background-color: #282c34;
color: #787a80;
}

code {
font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New",
monospace;
}
}
```
* Go to the Todo directory: `cd ../..`
* When you are in the Todo directory run: `npm run dev`

**Assuming no errors when saving all these files, our To-Do app should be ready and fully functional with the functionality discussed earlier: creating a task, deleting a task and viewing all your tasks.**
![MyTodo AppWebpage](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/60b239b8-c003-43ea-8ecf-ae9adf1cef3e)

### **Thank You.**
