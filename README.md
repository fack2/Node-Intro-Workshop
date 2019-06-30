# Node-Intro-Workshop
JavaScript was designed for the browser and does not support writing server-side code out of the box.

### What is Node?

NodeJS is a run-time environment that allows you to develop the server side of your application using JavaScript. 
What makes NodeJS unique and especially useful to a full-stack developer is that you can use the same language on the client side (browser), and server-side (back-end). 

It doesn't have:

* a DOM (hallelujah)
* a window object, or access to browser APIs


The command-line command `node path_to_program.js` allows you to execute JavaScript programs you've
written (that don't use browser features) and will print what the program returns to the console. 

### What is a server?

What is a server? What's a front-end and back-end?

the front and back end run in different environments, and they communicate through the url. Client-side JavaScript runs in the visitor's browser, whereas server-side code runs on a website's web server.

A server is a computer program that receives requests from other programs, the client, and sends back a response, e.g. to share data, information or hardware and software resources.

In a typical web app a server could perform some of these functions:

* Handle manipulation of data in the database
* File manipulation
* Authentication
* Secret Logic
* Client side sends requests to a server which sends back data to the front end to be displayed



### How Node works

Node handles I/O with: callbacks, events, streams and modules

This article presents a great summary of these topics and Node as a whole: [the art of Node](https://github.com/maxogden/art-of-Node).

### Modules

There are three types of modules in Node.js:

* core modules - the Node core library is made up of about two dozen modules, some lower level like events and stream some higher like http. They come installed with Node automatically.

We will mainly be using the following core modules this week:

* fs
* http
* path
* querystring
* url

There are also other types of modules:

* third-party modules - there are thousands of open-source, 3rd-party Node modules created by other people.
* your own modules!


### http server

We now have everything we need to start writing Node code in this repo and create our first http server!

The next part of this tutorial will be done as a code along.
