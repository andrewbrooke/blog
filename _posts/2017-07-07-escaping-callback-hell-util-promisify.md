---
layout: post
title: Escaping Callback Hell with util.promisify
author: Andrew Brooke
comments: true
---

[Callback hell](http://callbackhell.com/) - as it's unaffectionately referred to by programmers - is a JavaScript developer's worst nightmare. It's common among newbie developers as they begin programming asynchronous JavaScript - and it's unpleasant to look at, at the very least.

Here's an example of what I mean,

#### hell.js
<pre>
  <code class="javascript">
fs.readdir(__dirname, (err, files) => {
    if (err) return console.error(err);
    files.forEach((file, index) => {
        fs.stat(file, (err, stats) => {
            if (err) return console.error(err);
            if (stats.isFile()) {
                fs.readFile(file, 'utf8', (err, text) => {
                    if (err) return console.error(err);
                    fs.writeFile('./' + file + '.bak', text, (err) => {
                        if (err) return console.error(err);
                        console.log('Wrote file!');
                    });
                });
            }
        });
    });
});
  </code>
</pre>

While basically useless if you follow along what this chunk of code does, it's also particularly ugly.

As of Node.js 8, there is default support for a new API called `util.promisify`, which can be advantageous in a scenario like the aforementioned one.

Let's try a basic example to see how it works, then try to refactor that nasty code.

#### example.js
<pre>
  <code class="javascript">
const util = require('util');
const fs = require('fs');

const readdir = util.promisify(fs.readdir);
readdir(__dirname).then((files) => {
    console.log(files);
}).catch((err) => {
    console.log(err);
});
  </code>
</pre>

This line in particular is the most interesting, `const readdir = util.promisify(fs.readdir);`

According to the [Node.js official docs](https://nodejs.org/api/util.html#util_util_promisify_original),

> **util.promisify** Takes a function following the common Node.js callback style, i.e. taking a (err, value) => ... callback as the last argument, and returns a version that returns promises.

Great, so we can get rid of all of those horrible callbacks in favor of promises. Let's try it out!

#### heaven.js
<pre>
  <code class="javascript">
const util = require('util');
const fs = require('fs');

// Loop through keys of fs module and promisify all functions
Object.keys(fs).forEach((key) => {
    if (typeof fs[key] === 'function') {
        fs[key] = util.promisify(fs[key]);
    }
});

// Destructure fs functions into the ones we will be using
let { readdir, stat, readFile, writeFile } = fs;

async function heaven() {
    let files = await readdir(__dirname);
    for (let file of files) {
        let stats = await stat(file);
        if (stats.isFile()) {
            let text = await readFile(file, 'utf8');
            writeFile('./' + file + '.bak', text);
            console.log('Wrote file!');
        }
    }
}

heaven().then(() => {
    console.log('Wrote all files!');
}).catch((err) => {
    console.log(err);
});
  </code>
</pre>

While still a useless function, it's looking a lot nicer now, all thanks to the magic of `util.promisify` and some `async/await` thrown in.

This new API can also be used in situations where promisified function does not follow the error-first callback style, making it even more generalizable.

See the [Node.js docs](https://nodejs.org/api/util.html#util_custom_promisified_functions) for more details about that!
