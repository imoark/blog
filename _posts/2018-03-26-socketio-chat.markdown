---
layout: post
title:  "SocketIO Chat"
date:   2018-03-26 07:29:04 -0800
categories: jekyll update
---

Today, I am going to talk about SocketIO. It is one of the most powerful JavaScript frameworks that enables real-time bidirectional event-based communication. In a simple word, **you can make a chat app out of it**.
It works on every platform, browser or device, focusing equally on reliability and speed.

## Get Started: Chat application
In this guide we’ll create a basic chat application. It requires almost no basic prior knowledge of Node.JS or Socket.IO, so it’s ideal for users of all knowledge levels.

## Introduction
Back in the past, coding a chat application with web applications stacks such as PHP has traditionally been a pain in the arse. It involves polling the server for changes, keeping track of timestamps, and it’s a lot slower than it should be.

Sockets technology have traditionally been the solution around which most realtime chat systems are architected, providing a bi-directional communication channel between a client and a server.

This means that the server can push messages to clients. Whenever you write a chat message, the idea is that the server will get it and push it to all other connected clients.

I have provided you the finished code in a repository [https://github.com/imoark/chatsocket.git](https://github.com/imoark/chatsocket.git). While you can just grab my code and run the app, I will walk you through line by line of the code. So get started.

## The framework
The first goal is to setup a simple HTML webpage that serves out a form and a list of messages. We’re going to use a Node.JS web framework `express` to this end. Make sure Node.JS is installed.

First let’s see the package.json manifest file that describes our project. We just need 2 modules, `express` and `socketio` npm module. Don't forget to initialize `start` to run `node index.js`. So, by running `npm start` on your console later, we will execute `index.js` later on.

```
{
  "name": "socket.io-chat",
  "version": "1.0.0",
  "description": "A simple chat client using socket.io",
  "main": "index.js",
  "author": "Mario Kurniawan",
  "private": true,
  "license": "BSD",
  "dependencies": {
    "express": "4.13.4",
    "socket.io": "^1.7.3"
  },
  "scripts": {
    "start": "node index.js"
  }
}

```

Now, in order to easily populate the module with the things we need, we’ll use `npm install` or `npm i`.

```
$ npm i
```

Now that `express` is installed we can create an `index.js` file that will setup our application.

```javascript
var express = require('express');
var app = express();
var server = require('http').createServer(app);

app.get('/', function(req, res){
  res.send('<h1>Hello world</h1>');
});

server.listen(3000, function(){
  console.log('listening on *:3000');
});
```
    
This translates into the following:

1. Express initializes app to be a function handler that you can supply to an HTTP server (as seen in line 2).
2. We define a route handler / that gets called when we hit our website home.
3. We make the http server listen on port 3000.

If you run `node index.js` you should see the following:

![Terminal index.js]({{ "/assets/img/cap25.PNG" | absolute_url }})

And if you point your browser to `http://localhost:3000:`

![Localhost index.js]({{ "/assets/img/cap26.PNG" | absolute_url }})

##Serving HTML
So far in index.js we’re calling res.send and pass it a HTML string. Our code would look very confusing if we just placed our entire application’s HTML there. Instead, we’re going to create a index.html file and serve it.

Let’s refactor our route handler to route to `/public` folder instead:

```javascript
// Routing
app.use(express.static(path.join(__dirname, 'public')));
```
    
And populate `/public/index.html` with the following:

```HTML
<!doctype html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Socket.IO Chat Example</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <ul class="pages">
    
    // and in /public/main.js, after they authenticate your user name,
    // login page will fadeout() and chat page will show up.
    <li class="chat page">
      <div class="chatArea">
        <ul class="messages"></ul>
      </div>
      <input class="inputMessage" placeholder="Type here..."/>
    </li>

    // This will load first.....
    <li class="login page">
      <div class="form">
        <h3 class="title">What's your nickname?</h3>
        <input class="usernameInput" type="text" maxlength="14" />
      </div>
    </li>
  </ul>

  <script src="https://code.jquery.com/jquery-1.10.2.min.js"></script>
  <script src="/socket.io/socket.io.js"></script>
  <script src="/main.js"></script>
</body>
</html>
```

In the `/public` folder, we are gonna be doing a lot of front-end stuff. We put some `css` to give some good visual on the chat UI, and `main.js` will be where the most of the animation, sequence, data passing, security constrain (prevents input from having injected markup), setting events, etc. 

I will talk in details about the `setting events` part, because that one has something to do with the `socket.io` module that we are using. But in the meantime, for the other general stuff of `main.js`, I've commented each of the function with its primary purposes, so feel free to walkthrough it by yourself.


## Integrating Socket.IO
Socket.IO is composed of two parts:

1. A server that integrates with (or mounts on) the Node.JS HTTP Server: `socket.io`
2. A client library that loads on the browser side: `socket.io-client`, which in this part, we'll just put that one in the HTML as 
> `<script src="/socket.io/socket.io.js"></script>`

During development, `socket.io` serves the client automatically for us.
So now, the refined `index.js` would look like this.

```javascript
var express = require('express');
var app = express();
var path = require('path');
var server = require('http').createServer(app);
var io = require('socket.io')(server);
var port = process.env.PORT || 3000;

// Routing
app.use(express.static(path.join(__dirname, 'public')));

io.on('connection', function(socket){
  console.log('a user connected');
});

server.listen(port, function () {
  console.log('Server listening at port %d', port);
});
```
    
Notice that I initialize a new instance of `socket.io` by passing the `http` (the HTTP server) object. Then I listen on the `connection` event for incoming sockets, and I log it to the console.

Now in `index.html` I add the following snippet before the `</body>`:

```
<script src="/socket.io/socket.io.js"></script>
<script>
  // this part in the final code will be put in /public/main.js
  var socket = io();
</script>
```
    
That’s all it takes to load the `socket.io-client`, which exposes a `io` global, and then connect.

Notice that I’m not specifying any URL when I call `io()` , since it defaults to trying to connect to the host that serves the page.

If you now reload the server and the website you should see the console print “a user connected”.
Try opening several tabs, and you’ll see several messages:

![Console Log connect]({{ "/assets/img/cap27.PNG" | absolute_url }})

Each socket also fires a special disconnect event:

```javascript
io.on('connection', function(socket){
  console.log('a user connected');
  socket.on('disconnect', function(){
    console.log('user disconnected');
  });
});
```
    
Then if you refresh a tab several times you can see it in action:

![Console Log disconnect]({{ "/assets/img/cap28.PNG" | absolute_url }})

## Emitting events & Broadcasting

After doing that simple demonstration, we will need to focus on the key broadcast that our chat app will do. Here are the list:
- [ ]  Client emits 'new message'. The app will listens and **executes** `new message'
- [ ]  Client emits 'add user'. The app will listens and **executes** storing the username in the socket session for this client, **echo** globally (all clients) that a person has connected
- [ ]  Client emits 'typing'. The app will **broadcast** to other
- [ ]  Client emits 'stop typing'. The app will **broadcast** to other
- [ ]  Client emits 'disconnect'. The app will **broadcast** to other that this client has left

So, here is the code part:
```javascript
io.on('connection', function (socket) {
  var addedUser = false;

  // when the client emits 'new message', this listens and executes
  socket.on('new message', function (data) {
    // we tell the client to execute 'new message'
    socket.broadcast.emit('new message', {
      username: socket.username,
      message: data
    });
  });

  // when the client emits 'add user', this listens and executes
  socket.on('add user', function (username) {
    if (addedUser) return;

    // we store the username in the socket session for this client
    socket.username = username;
    ++numUsers;
    addedUser = true;
    socket.emit('login', {
      numUsers: numUsers
    });
    // echo globally (all clients) that a person has connected
    socket.broadcast.emit('user joined', {
      username: socket.username,
      numUsers: numUsers
    });
  });

  // when the client emits 'typing', we broadcast it to others
  socket.on('typing', function () {
    socket.broadcast.emit('typing', {
      username: socket.username
    });
  });

  // when the client emits 'stop typing', we broadcast it to others
  socket.on('stop typing', function () {
    socket.broadcast.emit('stop typing', {
      username: socket.username
    });
  });

  // when the user disconnects.. perform this
  socket.on('disconnect', function () {
    if (addedUser) {
      --numUsers;

      // echo globally that this client has left
      socket.broadcast.emit('user left', {
        username: socket.username,
        numUsers: numUsers
      });
    }
  });
});
```

Another important section, is about the primary `Emitting Events`. Here are the list:
- [ ]  Server emits 'login', log the login (welcome) message
- [ ]  Server emits 'new message', update the chat body
- [ ]  Server emits 'user joined', log it in the chat body
- [ ]  Server emits 'user left', log it in the chat body
- [ ]  Server emits 'typing', show the typing message
- [ ]  Server emits 'stop typing', kill the typing message


```javascript
// Socket events

  // Whenever the server emits 'login', log the login message
  socket.on('login', function (data) {
    connected = true;
    // Display the welcome message
    var message = "Welcome to Socket.IO Chat – ";
    log(message, {
      prepend: true
    });
    addParticipantsMessage(data);
  });

  // Whenever the server emits 'new message', update the chat body
  socket.on('new message', function (data) {
    addChatMessage(data);
  });

  // Whenever the server emits 'user joined', log it in the chat body
  socket.on('user joined', function (data) {
    log(data.username + ' joined');
    addParticipantsMessage(data);
  });

  // Whenever the server emits 'user left', log it in the chat body
  socket.on('user left', function (data) {
    log(data.username + ' left');
    addParticipantsMessage(data);
    removeChatTyping(data);
  });

  // Whenever the server emits 'typing', show the typing message
  socket.on('typing', function (data) {
    addChatTyping(data);
  });

  // Whenever the server emits 'stop typing', kill the typing message
  socket.on('stop typing', function (data) {
    removeChatTyping(data);
  });

  socket.on('disconnect', function () {
    log('you have been disconnected');
  });

  socket.on('reconnect', function () {
    log('you have been reconnected');
    if (username) {
      socket.emit('add user', username);
    }
  });

  socket.on('reconnect_error', function () {
    log('attempt to reconnect has failed');
  });
```

## Final Words

So, in the end, you got your Chat App working about now. Even though it is already fun to see it running in local, the excitement begins when you deploy this online, and users who visit that link can chat altogether. I run my ChatAPP online in this link [https://socketdemochatapp.herokuapp.com/](https://socketdemochatapp.herokuapp.com/) and you can enjoy chatting with your friend now. Cheers!