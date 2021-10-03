---
layout: post
title: My Node.js Cheat Sheet
subtitle: Bunch of Node.js codes
cover-img: /assets/img/node-js-background.jpg
thumbnail-img: /assets/img/node-js.png
share-img: /assets/img/path.jpg
tags: [development]
permalink: "/:title"
---
[Node.js](https://en.wikipedia.org/wiki/Node.js) is a JavaScript Runtime that allows us to run JavaScript code on the server. It is written in C++ and built on [V8 JavaScipt engine](https://en.wikipedia.org/wiki/V8_(JavaScript_engine)). Node.js is fast, efficient and highly scalable and popular in the industry. We can work with REST API & Microservices, Real Time Services and CRUD Apps with Node.js. Also Node packages come really handy and easy to install.

We can check if our system has node installed with:

```
node --version
```

**Path Module**:

```
const path = require('path');

// Base file name
console.log(path.basename(__filename));

// Directory name
console.log(path.dirname(__filename));

// File extension
console.log(path.extname(__filename));

// Create path object
console.log(path.parse(__filename).base);

// Concatenate paths
console.log(path.join(__dirname, 'test', 'hello.html'));
```

---

**OS Module**:

```
const os = require('os');

// Platform
console.log(os.platform());

// CPU Arch
console.log(os.arch());

// CPU Core Info
console.log(os.cpus());

// Free memory
console.log(os.freemem());

// Total memory
console.log(os.totalmem());

// Home dir
console.log(os.homedir());

// Uptime
console.log(os.uptime());
```

---

**URL Module**:

```
const url = require('url');
const {log} = require("nodemon/lib/utils");

const myUrl = new URL('http://mywebsite.com/hello.html?id=100&status=active');

// Serialized URL
console.log(myUrl.href);
console.log(myUrl.toString());

// Host (root domain)
console.log(myUrl.host);

// Hostname (does not get port)
console.log(myUrl.hostname);

// Path name
console.log(myUrl.pathname);

// Serialized query
console.log(myUrl.search);

// Params object
console.log(myUrl.searchParams);

// Add param
myUrl.searchParams.append('abc', '123');
console.log(myUrl.searchParams);

// Loop through params
myUrl.searchParams.forEach((value, name) => console.log(`${name}: ${value}`));
```

---

**Event Module**:

```
const EventEmitter = require('events');

// Create class
class MyEmitter extends EventEmitter {
}

// Init object
const myEmitter = new MyEmitter();

// Event listener
myEmitter.on('event', () => console.log('Event Fired!'));

// Init event
myEmitter.emit('event');
myEmitter.emit('event');
myEmitter.emit('event');
```

---

**Logger With Event Emiiter**

*index.js*
```
const Logger = require('./logger');

const logger = new Logger();

logger.on('message', data => console.log('Called Listener: ', data));

logger.log('Hello World');
logger.log('Hello');
logger.log('Hi');
```

*logger.js*
```
const EventEmitter = require('events');
const uuid = require('uuid');

// console.log(uuid.v4());

class Logger extends EventEmitter {
    log(msg) {
        // Call event
        this.emit('message', { id: uuid.v4(), msg });
    }
}

module.exports = Logger;
```

---

**HTTP Module**
```
const http = require('http');

// Create server object
http.createServer((req, res) => {
    res.write('Hello World');
    res.end();
}).listen(5000, () => console.log('Server running...'));
```

---

**Create a Server**

index.js create server function
```
const server = http.createServer((req, res) => {
    if (req.url === '/') {

        fs.readFile(path.join(__dirname, 'public', 'index.html'), (error, content) => {
            if (error) {
                throw error;
            }
            res.writeHead(200, {'Content-Type': 'text/html'});
            res.end(content);
        });

    }
});
```

index.js
```
const http = require('http');
const path = require('path');
const fs = require('fs');

const server = http.createServer((req, res) => {

    // Build file path
    let filePath = path.join(__dirname, 'public', req.url === '/' ? 'index' : req.url);

    // Exntension of file
    let extname = path.extname(filePath);

    // Initial content type
    let contentType = 'text-html';

    // Check ext and set content type
    switch (extname) {
        case '.js':
            contentType = 'text/javascript';
            break;
        case '.css':
            contentType = 'text/css';
            break;
        case '.json':
            contentType = 'application/json';
            break;
        case '.png':
            contentType = 'image/png';
            break;
        case '.jpg':
            contentType = 'image/jpg';
            break;
    }

    // Read File
    fs.readFile(filePath, (err, content) => {
        if (err) {
            if (err.code === 'ENOENT') {
                // Page not found
                fs.readFile(path.join(__dirname, 'public', '404.html'), (err, content) => {
                    res.writeHead(200, {'Content-Type': 'text/html'});
                    res.end(content, 'utf8');
                });
            } else {
                // Some server error
                res.writeHead(500);
                res.end(`Server Error: ${err.code}`);
            }
        } else {
            // Success
            res.writeHead(200, {'Content-Type': contentType});
            res.end(content, 'utf8');
        }
    });
});

// if host's port choice not found in env variable 5000
const PORT = process.env.PORT || 5000;

server.listen(PORT, () => console.log(`Server running on port ${PORT}`));
```

---

# Deploy to Heroku

[Heroku](https://dashboard.heroku.com/apps/fathomless-bastion-01825/deploy/heroku-git)

**Install the Heroku CLI**

Download and install the Heroku CLI.

If you haven't already, log in to your Heroku account and follow the prompts to create a new SSH public key.

```
$ heroku login
```

**Clone the repository**

Use Git to clone fathomless-bastion-01825's source code to your local machine.

```
$ heroku git:clone -a fathomless-bastion-01825
$ cd fathomless-bastion-01825
```

**Deploy your changes**

Make some changes to the code you just cloned and deploy them to Heroku using Git.

```
$ git add .
$ git commit -am "make it better"
$ git push heroku master
```

Thanks for checking it out!
