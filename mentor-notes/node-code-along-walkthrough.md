# Node intro workshop notes
## Step 1 - create server that sends plain text to every endpoint
1. clone this repo and run `npm init -y` 
2. first open `src` 
  * `src` (this is where we will build our server)


```js
// 1. 'requiring' a module makes that module available 
const http = require('http'); 


// 2. we need a host and a port. On a local server the host is 
// allocated automatically (to localhost). We specify a port to 
// make sure our server only "listens" to things coming through 
// that port
const port = 4000; 


// 4. define the router it takes two arguments. request and response; 
// `request` is an object with information about the request that has 
// been sent to you. `response` is an object with methods on it which 
// allow you to send things back to the user.
// functions declared with `const` are not hoisted and so need to be 
// defined before they are used
const router = (request, response) => {
// the end method on the response object sends the string and ends the
// connection that has been opened between the client and the server.
  response.end('hello world');
};

// 3. create a server, 
const server = http.createServer(router) 

// 5. tell the server to listen to the port specified
server.listen(port);

// 6. add console.log to let the user know the server is now running
console.log(`server up and running on localhost:${port}`);
```
  

run the server by running ```node src/server.js```
  * navigate to `localhost:4000` in your browser
  * show that the plaintext we typed in is now coming up in our browser
  * show that no matter what path you give to `localhost:4000` (eg. `localhost:4000/yolo`) you still get the same response
  * this is because currently in our router function we don't look at the request, we just send the same response no matter what





## Step 2 - create different endpoints and 404

```js
const http = require('http');

const port = 4000;

const router = (request, response) => {
    // 1. add url variable, this will be a string with whatever is 
    // in the url bar after localhost:4000
  const url = request.url;
  
    // 2. add conditions for '/' and '/yolo'. 
  if (url === '/') {
    response.end('hello world');
  } else if (url === '/yolo') {
    response.end('this is yolo');
  } 

};

const server = http.createServer(router);

server.listen(port);

console.log(`server up and running on localhost:${port}`);
```



show what happens in browser at these endpoints ('/' and 'yolo'), then go to an unspecified endpoint. this is because the client has opened a connection to the server and is waiting for a response, since our router function does not send anything back, or end the connection, the client is just left 'hanging', until the browser gets bored of waiting and closes the connection


```js

  if (url === '/') {
  // 4. add writehead
    response.writeHead(200)
    
    response.end('hello world');
  } else if (url === '/yolo') {
  
  // 4. add writehead
    response.writeHead(200)
    
    response.end('this is yolo');
  } 
  // 3. add an else with writehead
else {
    response.writeHead(404);
    response.end('404 not found');
  }

```



## Step 3 - move router to separate file
now this function is getting a bit big, so it's a good time to make our code a bit more modular. 
1. create a file in src called router.js, copy and paste the router function to this new file and delete it from server.js

2. put ```module.exports = router``` at the bottom of the file

3. go back to server.js and require in the router file
```const router = require('./router');```

if you are requiring in a node module (either one from core or one you downloaded from `npm`) you just write the name, if you are requiring in a local file (that you've used module.exports in) then put './' before the file path (relative to file you are requiring it into).

4. restart your server again and make sure everything stil works
5. install nodemon `npm i nodemon -D`so that you dont have to restart the server every time you change something, then create a script and restart server
```js
"start:watch" : "nodemon src/server.js " 
```

## Step 4 - send files

in router.js:

```js
 if (url === '/') {
     // 1. fs.readFileSync takes a file path and parses the html file in the way we need
    const html = fs.readFileSync(
    // 2. path.join joins different sections of a file path, 
    // __dirname refers to the folder you are currently in
      path.join(__dirname, '..', 'public', 'index.html')
    );
    response.writeHead(200, { 'Content-Type': 'text/html' });
    response.end(html);
  } 
```
go to the browser and see you now have html 


## Step 5 replace readFileSync with readFile and update errors
1. update this to use `readFile` rather than `readFileSync` so that we can read files asynchronously. it takes the file path and an error first callback.
```
const filePath = path.join(__dirname, '..', 'public', 'index.html');
    fs.readFile(filePath, (error, file) => {
      if (error) {
        console.log(error);
      } else {
        response.writeHead(200, { 'Content-Type': 'text/html' });
        response.end(file);
      }
 ```
2. update the 404 route 
```js 
else {
    response.writeHead(404, { 'Content-Type': 'text/html' });
    response.end('<h1>404 not found</h1>');
  }
 ``` 

3. update the server error section 

```js
 if (error) {
        console.log(error);
        response.writeHead(500, { 'Content-Type': 'text/html' });
        response.end("<h1>Sorry, we've had a problem on our end</h1>");
      }
```

4. delete yolo route


## Step 6 - serving css, index.js and faviiiiiicon


```js
//1. add else if conditon for css

else if (url === '/public/main.css') {
    const filePath = path.join(__dirname, '..', url);
    fs.readFile(filePath, (error, file) => {
      if (error) {
        console.log(error);
        response.writeHead(500, { 'Content-Type': 'text/html' });
        response.end("<h1>Sorry, we've had a problem on our end</h1>");
      } else {
        response.writeHead(200, { 'Content-Type': 'text/css' });
        response.end(file);
      }
    });
  } else if (url === '/public/index.js') {
    const filePath = path.join(__dirname, '..', url);
    fs.readFile(filePath, (error, file) => {
      if (error) {
        console.log(error);
        response.writeHead(500, { 'Content-Type': 'text/html' });
        response.end("<h1>Sorry, we've had a problem on our end</h1>");
      } else {
        response.writeHead(200, { 'Content-Type': 'application/javascript' });
        response.end(file);
      }
    });
  } else if (url === '/public/node-icon.ico') {
    const filePath = path.join(__dirname, '..', url);
    fs.readFile(filePath, (error, file) => {
      if (error) {
        console.log(error);
        response.writeHead(500, { 'Content-Type': 'text/html' });
        response.end("<h1>Sorry, we've had a problem on our end</h1>");
      } else {
        response.writeHead(200, { 'Content-Type': 'image/x-icon' });
        response.end(file);
      }
    });
  }
  ```
 
## Step 7 - refactor

change all the else if's into this: 

```js
// 1 -  check if the url has a public in it
else if (url.includes('public')) {
    // 2 - check the file extension by splitting
    const extension = url.split('.')[1];
    // 3 - list extension types
    const extensionType = {
      html: 'text/html',
      css: 'text/css',
      js: 'application/javascript',
      ico: 'image/x-icon',
    };

    const filePath = path.join(__dirname, '..', url);
    fs.readFile(filePath, (error, file) => {
      if (error) {
        console.log(error);
        response.writeHead(404, { 'Content-Type': 'text/html' });
        response.end('<h1>404 file not found</h1>');
      } else {
        response.writeHead(200, { 'Content-Type': extensionType[extension] });
        response.end(file);
      }
    });
    console.log(url);
  }
  
 ```
  


## Step 8 - modularising

move all logic from router to a separate file called handler.js so that router.js just contains the logic of which route goes where.

handlers.js

```js

const fs = require('fs');
const path = require('path');

const handleHomeRoute = (request, response) => {
  const filePath = path.join(__dirname, '..', 'public', 'index.html');
  fs.readFile(filePath, (error, file) => {
    if (error) {
      console.log(error);
      response.writeHead(500, { 'Content-Type': 'text/html' });
      response.end("<h1>Sorry, we've had a problem on our end</h1>");
    } else {
      response.writeHead(200, { 'Content-Type': 'text/html' });
      response.end(file);
    }
  });
};

const handlePublic = (request, response, url) => {
  const extension = url.split('.')[1];
  const extensionType = {
    html: 'text/html',
    css: 'text/css',
    js: 'application/javascript',
    ico: 'image/x-icon',
  };

  // Replaced err with error in line 30
  const filePath = path.join(__dirname, '..', url);
  fs.readFile(filePath, (error, file) => {
    if (error) {
      console.log(error);
      response.writeHead(404, { 'Content-Type': 'text/html' });
      response.end('<h1>404 file not found</h1>');
    } else {
      response.writeHead(200, { 'Content-Type': extensionType[extension] });
      response.end(file);
    }
  });
  console.log(url);
};

module.exports = {
  handleHomeRoute,
  handlePublic,
};


```

router.js: 

```js 

const handlers = require('./handlers.js');

const router = (request, response) => {
  const url = request.url;
  if (url === '/') {
    handlers.handleHomeRoute(request, response);
  } else if (url.includes('public')) {
    handlers.handlePublic(request, response, url);
  } else {
    response.writeHead(404, { 'Content-Type': 'text/html' });
    response.end('<h1>404 file not found</h1>');
  }
};

module.exports = router;

```

* we don't modularise the 404 functionality as it is only two lines, to add it to our handlers file we would be adding more lines of code, without making anything clearer.
