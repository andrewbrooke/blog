---
layout: post
title: Building a Realtime Chat with Node and Sockets (Part 1 - Background and Project Setup)
author: Andrew Brooke
comments: true
github_repo: SyntonicApps/node-socket-chat
---

One of the most common starter NodeJS projects is a simple chat app, often utilizing websockets for realtime functionality. So, do we need another chat tutorial for Node? Why exactly am I writing this? When I first started learning Node, I began with a similar project to get my feet wet. It left me wanting more, so this tutorial will serve as an extension of the basic chat application, with some additional functionality.

By the end of this tutorial we will complete the following:

- Server side code with [NodeJS](https://nodejs.org/en/), [Express](http://expressjs.com/), and [socket.io](http://socket.io/)
- Database for chat persistence with [RethinkDB](https://www.rethinkdb.com/)
- Frontend UI with [AngularJS](https://angularjs.org/)
- Unit tests with [Mocha](https://mochajs.org/)

That's a lot of stuff, and hopefully by the time our app is complete you'll learn something new. Let's get started!

# File Structure

In an empty directory, you should create the following files and directories (don't worry about their contents for now)
<pre>
	<code class="bash">
|- public
	...
|- test
	...
app.js
	</code>
</pre>

The `public` directory will contain all of the files related to the client, and anything to do with AngularJS. `test` will contain our Mocha unit tests. `app.js` is the main NodeJS file, and will contain all of the server-side code.

# Project Setup

If you don't already have it, install [NodeJS](https://nodejs.org/en/). Preferably you should install Node 6.8.1 or higher, as this tutorial will be using ES6 style syntax.

NodeJS comes bundled with npm, a package manager for JavaScript. This is what we will be using to manage our project. Every npm package needs a file called `package.json`, usually located in your project's root directory. This file contains all of the metadata about a package, including the other packages it has as dependencies. You can generate a `package.json` by running `npm init` on the command line, or copy and paste this as a template.

<pre>
	<code class="javascript">
{
  "name": "node-socket-chat",
  "version": "1.0.0",
  "description": "A chat app built with NodeJS, Socket.io, RethinkDB, and AngularJS",
  "main": "app.js",
  "scripts": {
    "start": "node app",
    "test": "mocha test"
  },
  "author": "Andrew Brooke <andrewbrooke15@gmail.com> (https://andrewbrooke.github.io)",
  "license": "ISC",
  "repository": "https://github.com/SyntonicApps/node-socket-chat"
}
	</code>
</pre>

Feel free to change the `name`, `description`, `author`, and `repository` fields to whatever you like.

Now, open up a command line and run the following commands to install our project's dependencies

<pre>
	<code class="bash">
npm install --save socket.io rethinkdb express
npm install --save-dev mocha
	</code>
</pre>

That's it for Part 1. In the next segment we'll start coding on the server components.