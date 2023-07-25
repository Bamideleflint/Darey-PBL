# MEAN STACK DEPLOYMENT TO UBUNTU IN AWS

####  MEAN Stack is a combination of the following components:
**MEAN STACK = MongoDB + ExpressJS + AngularJS + NodeJS**
1. **MongoDB** (Document database) – Stores and allows to retrieve data.
2. **Express** (Back-end application framework) – Makes requests to Database for Reads and Writes.
3. A**ngular** (Front-end application framework) – Handles Client and Server Requests
4. **Node.js** (JavaScript runtime environment) – Accepts requests and displays results to end user

In order to complete the project, we need to spin up an instance in our **AWS** account and connect to a terminal.

Our task is to implement a simple Book Register web form using MEAN stack.

**Step 1**: Install NodeJs
Node.js is a JavaScript runtime built on Chrome’s V8 JavaScript engine. Node.js is used in this tutorial to set up the Express routes and AngularJS controllers.

Update ubuntu: `sudo apt update`

Upgrade ubuntu: `sudo apt upgrade -y`

Add certificates: 
```
sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates

curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -
```

![AddCertificates](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/b4d9971d-4654-406f-834a-7c79fafd9ff8)


Install NodeJS: `sudo apt install -y nodejs`

**Step 2**: Install MongoDB
MongoDB stores data in flexible, JSON-like documents. Fields in a database can vary from document to document and data structure can be changed over time. For our example application, we are adding book records to MongoDB that contain book name, isbn number, author, and number of pages.

Import the Public Key used by Package Management System: `sudo apt-get install gnupg curl`


`curl -fsSL https://pgp.mongodb.com/server-6.0.asc | sudo gpg -o /usr/share/keyrings/mongodb-server-6.0.gpg --dearmor`

Create a List file For MongoDB

**For Ubuntu 20.04**
`echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-6.0.gpg ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list`

**For Ubuntu 22.04**

`echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-6.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list`

Reload Local Packages: `sudo apt-get update`

Install MongoDB: `sudo apt-get install -y mongodb-org`

Start and Enable The Mongod Service:
```
sudosystemctl start mongod

sudosystemctl enable mongod
```

Verify that the service is up and running: `sudo systemctl status mongod`
![MongodbRunning](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/3a64db99-8a89-4d97-98d5-508473880e61)

We need ‘body-parser’ package to help us process JSON files passed in requests to the server.

Install body-parser package: `sudo npm install body-parser`
![BodyParser](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/bfc3778e-9800-470a-83f9-88a1540d843a)

Create a folder named ‘Books’: `mkdir Books && cd Books`

In the Books directory, Initialize npm project: `npm init`

Add a file to it named server.js: `vi server.js`

Copy and paste the web server code below into the server.js file.

```
var express = require('express');
var bodyParser = require('body-parser');
var app = express();
app.use(express.static(__dirname + '/public'));
app.use(bodyParser.json());
require('./apps/routes')(app);
app.set('port', 3300);
app.listen(app.get('port'), function() {
    console.log('Server up: http://localhost:' + app.get('port'));
});
```

#### **INSTALL EXPRESS AND SET UP ROUTES TO THE SERVER**
**Step 3**: Install **Express** and set up routes to the server
**Express** is a minimal and flexible Node.js web application framework that provides features for web and mobile applications. We will use **Express** in to pass book information to and from our **MongoDB database.**

We also will use **Mongoose** package which provides a straight-forward, schema-based solution to model your application data. We will use **Mongoose** to establish a schema for the database to store data of our book register.

`sudo npm install express mongoose`

In ‘Books’ folder, create a folder named apps: `mkdir apps && cd apps`

Create a file named routes.js: `vi routes.js`

Copy and paste the code below into routes.js
```
const Book = require('./models/book');

module.exports = function(app){
  app.get('/book', function(req, res){
    Book.find({}).then(result => {
      res.json(result);
    }).catch(err => {
      console.error(err);
      res.status(500).send('An error occurred while retrieving books');
    });
  });

  app.post('/book', function(req, res){
    const book = new Book({
      name: req.body.name,
      isbn: req.body.isbn,
      author: req.body.author,
      pages: req.body.pages
    });
    book.save().then(result => {
      res.json({
        message: "Successfully added book",
        book: result
      });
    }).catch(err => {
      console.error(err);
      res.status(500).send('An error occurred while saving the book');
    });
  });

  app.delete("/book/:isbn", function(req, res){
    Book.findOneAndRemove(req.query).then(result => {
      res.json({
        message: "Successfully deleted the book",
        book: result
      });
    }).catch(err => {
      console.error(err);
      res.status(500).send('An error occurred while deleting the book');
    });
  });

  const path = require('path');
  app.get('*', function(req, res){
    res.sendFile(path.join(__dirname, 'public', 'index.html'));
  });
};
```

In the ‘apps’ folder, create a folder named models: `mkdir models && cd models`

Create a file named book.js: `vi book.js`

Copy and paste the code below into ‘book.js’

```
var mongoose = require('mongoose');
var dbHost = 'mongodb://localhost:27017/test';
mongoose.connect(dbHost);
mongoose.connection;
mongoose.set('debug', true);
var bookSchema = mongoose.Schema( {
  name: String,
  isbn: {type: String, index: true},
  author: String,
  pages: Number
});
var Book = mongoose.model('Book', bookSchema);
module.exports = mongoose.model('Book', bookSchema);
```

**Step 4** – Access the routes with **AngularJS**

**AngularJS** provides a web framework for creating dynamic views in your web applications. In this tutorial, we use **AngularJS** to connect our web page with **Express** and perform actions on our book register.

Change the directory back to ‘Books’: `cd ../..`

Create a folder named public: `mkdir public && cd public`

Add a file named script.js: `vi script.js`

Copy and paste the Code below (controller configuration defined) into the script.js file.

```
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope, $http) {
  $http( {
    method: 'GET',
    url: '/book'
  }).then(function successCallback(response) {
    $scope.books = response.data;
  }, function errorCallback(response) {
    console.log('Error: ' + response);
  });
  $scope.del_book = function(book) {
    $http( {
      method: 'DELETE',
      url: '/book/:isbn',
      params: {'isbn': book.isbn}
    }).then(function successCallback(response) {
      console.log(response);
    }, function errorCallback(response) {
      console.log('Error: ' + response);
    });
  };
  $scope.add_book = function() {
    var body = '{ "name": "' + $scope.Name + 
    '", "isbn": "' + $scope.Isbn +
    '", "author": "' + $scope.Author + 
    '", "pages": "' + $scope.Pages + '" }';
    $http({
      method: 'POST',
      url: '/book',
      data: body
    }).then(function successCallback(response) {
      console.log(response);
    }, function errorCallback(response) {
      console.log('Error: ' + response);
    });
  };
});
```

In public folder, create a file named index.html: `vi index.html`

Cpoy and paste the code below into index.html file.

```
<!doctype html>
<html ng-app="myApp" ng-controller="myCtrl">
  <head>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
    <script src="script.js"></script>
  </head>
  <body>
    <div>
      <table>
        <tr>
          <td>Name:</td>
          <td><input type="text" ng-model="Name"></td>
        </tr>
        <tr>
          <td>Isbn:</td>
          <td><input type="text" ng-model="Isbn"></td>
        </tr>
        <tr>
          <td>Author:</td>
          <td><input type="text" ng-model="Author"></td>
        </tr>
        <tr>
          <td>Pages:</td>
          <td><input type="number" ng-model="Pages"></td>
        </tr>
      </table>
      <button ng-click="add_book()">Add</button>
    </div>
    <hr>
    <div>
      <table>
        <tr>
          <th>Name</th>
          <th>Isbn</th>
          <th>Author</th>
          <th>Pages</th>

        </tr>
        <tr ng-repeat="book in books">
          <td>{{book.name}}</td>
          <td>{{book.isbn}}</td>
          <td>{{book.author}}</td>
          <td>{{book.pages}}</td>

          <td><input type="button" value="Delete" data-ng-click="del_book(book)"></td>
        </tr>
      </table>
    </div>
  </body>
</html>
```

Change the directory back up to Books: `cd ..`

Start the server by running this command: `node server.js`
![serverjs running](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/1ca294b0-8512-4297-b684-cc68f275505e)
The server is now up and running, we can connect it via port 3300. You can launch a separate Putty or SSH console to test what curl command returns locally.

`curl -s http://localhost:3300`

It shall return an HTML page, it is hardly readable in the CLI, but we can also try and access it from the Internet.

For this – we need to open TCP port 3300 in our **AWS** Web Console, we need to edit the **inbound rule** in the **security group** of the **EC2** Instance we created.
![EditInboundRule3300](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/abf74ede-9b77-4be9-920e-fc1a5c27d56e)

Now we can access our **Book Register web application** from the Internet with a browser using **Public IP address or Public DNS name** of our **EC2** instance we created.
![Public IP](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/901ea0d5-af25-42e8-8573-0fca7c262905)

This is how our **Web Book Register Application** should look like in the browser:

![BookApp](https://github.com/Bamideleflint/Darey-PBL/assets/122679229/5aa4c1e5-5871-4094-92d7-d2aa7d8da3cb)


## **THANK YOU!**
