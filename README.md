How to build your own real-time chat app using NodeJS and AngularJS

Technology, Tools used
    Express, Angular, Node.
    Sockets to enable one-on-one communication in real time

Install Node.js
Installation on UNIX/Linux/Mac OS X, and SunOS
Based on your OS architecture, download and extract the archive node-v6.3.1-osname.tar.gz into /tmp, and then finally move extracted files into /usr/local/nodejs directory. For example:

$ cd /tmp
$ wget http://nodejs.org/dist/v6.3.1/node-v6.3.1-linux-x64.tar.gz
$ tar xvfz node-v6.3.1-linux-x64.tar.gz
$ mkdir -p /usr/local/nodejs
$ mv node-v6.3.1-linux-x64/* /usr/local/nodejs

Step 1 - Clone or Download the repository

$ git clone https://github.com/glokesh94/socket.io.git
$ cd socket.io

OR

Create the Root Folder 
C:\Users\username\Desktop>mkdir chat
C:\Users\username\Desktop>cd chat

Go to Server directory and run this command
C:\Users\username\Desktop\chat>sudo npm init
{
  "name": "chat",
  "version": "1.0.0",
  "description": "Chat application",
  "main": "server.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Your name",
  "license": "ISC"
}

Step 2 - Install the dependencies
socket.io — is a javascript library for real-time web applications. It enables real-time, bi-directional communication between web clients and servers.
express — is a Node.js web application framework. It provides the set of features to develop the web and mobile applications. One can respond to HTTP request using different middlewares and also render HTML pages.

C:\Users\username\Desktop\chat>sudo npm install --save socket.io
C:\Users\username\Desktop\chat>sudo npm install --save express

This will install required dependencies and add those to package.json. An extra field will be added to package.json which will look like this:

"dependencies": {
    "express": "^4.14.0",
    "socket.io": "^1.4.8"
}

Step 3 — Creating the Server
Create a server which serves at port 3000 and will send the html when called.
Each socket emits disconnect event which will be called whenever a client disconnects.
-> socket.on waits for the event. Whenever that event is triggered the callback function is called.
-> io.emit is used to emit the message to all sockets connected to it.

socket.on('event', function(msg){})
io.emit('event', 'message')

Create a server file with name server.js.
It can be added manually or using the command prompt.
C:\Users\username\Desktop\chat>sudo touch server.js
-> print a message to the console upon a user connecting
-> listen for chat message events and broadcast the received message to all sockets connected
-> Whenever a user disconnects, it should print the message to the console.

Copy this code in server.js
var app = require('express')();
var http = require('http').Server(app);
var io = require('socket.io')(http);

app.get('/', function(req, res){
  res.sendfile('index.html');
});

io.on('connection', function(socket){
  console.log('user connected');
  socket.on('chat message', function(msg){
    io.emit('chat message', msg);
  });
  socket.on('disconnect', function(){
    console.log('user disconnected');
  });
});

http.listen(3000, function(){
  console.log('listening on *:3000');
});

Building the Client
Create the index.html
It can be added manually or using the command prompt.
C:\Users\username\Desktop\chat>sudo touch index.html

Include socket.io-client and angular.js in your HTML script.
<script src = "https://ajax.googleapis.com/ajax/libs/angularjs/1.3.3/angular.min.js"></script>
<script src="/socket.io/socket.io.js"></script>

socket.io serves the client for us. It defaults to connect to the host that serves the page. Final HTML looks something like this:

Create an angular.js app and initialize a socket connection.
-> socket.on listens for a particular event. It calls a callback function whenever that event is called.
-> socket.emit is used to emit the message to the particular event.

Basic syntax of both are:

socket.on(‘event name’, function(msg){});
socket.emit('event name', message);

So whenever the message is typed and the button is clicked, call the function to send the message.
Whenever the socket receives a message, display it.
The JavaScript will look something like this:
var app=angular.module('myApp',[]);
	app.controller('mainController',['$scope',function($scope){
	  var socket = io.connect();
	  $scope.send = function(){
	  	socket.emit('chat message', $scope.message);
	  	$scope.message="";
	}
	socket.on('chat message', function(msg){
	  var li=document.createElement("li");
	  li.appendChild(document.createTextNode(msg));
	  document.getElementById("messages").appendChild(li);
	});
}]);

Running the application
Go to server directory where our server is present. Run the server using the following command:
C:\Users\username\Desktop\chat>sudo node server.js
The server starts running on port 3000. Go to the browser and type the following url:
http://localhost:3000