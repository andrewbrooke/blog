---
layout: post
title: Building a Reddit Bot with Node.js
author: Andrew Brooke
comments: true
github_repo: SyntonicApps/reddit-bot-example-node
---

If you frequent Reddit, odds are you've come across "Reddit Bot" users replying to posts and comments, and generally providing some pretty helpful functionality to the site. Many of these bots are built with Python and [PRAW](https://github.com/praw-dev/praw), but in this tutorial we'll be using our good pal Node.js to build our own bot.

The goal for this example bot is to look at an incoming stream of new comments posted on Reddit, and perform some simple logic to determine if we should send a response. If you want to build your own bot, this part is totally up to you, so use your imagination!

# Creating a Reddit App

First and foremost, you'll need to create a Reddit account that the bot will be posting from, so head on over to [Reddit](https://www.reddit.com) to do that. (If you have an existing account, you could use that, but typically developers will create fresh accounts specific to the bot they are making)

After you've done that, go to Reddit's [app preferences](https://www.reddit.com/prefs/apps/) and create your own application.

Make sure you provide a name and choose `script` for the type of the application. The redirect uri field is also required, but will not be used for this application, so you can put whatever URL you want.

![New App]({{ site.url }}/img/Reddit/new_app.PNG)

After creating the app, make a note of the app's client ID and secret ID, as you will need these later on.

![New App]({{ site.url }}/img/Reddit/ids.PNG)

# Creating the Node.js app

Grab Node.js and npm [here](https://nodejs.org/en/) if you don't have them already installed.

Pull up your favorite text editor and terminal, and get ready to enter the usual commands.

In a new directory, run `npm init` to create a new package.

Now let's install the packages we will be using in this tutorial.

First of all, we'll be using a package called [dotenv](https://www.npmjs.com/package/dotenv) to load up the sensitive environment variables for the Reddit app.

<pre>
  <code class="bash">
$ npm i -S dotenv
  </code>
</pre>

Next up, we'll be using [Snoowrap](https://www.npmjs.com/package/snoowrap) as a Node.js wrapper for the Reddit API, and [Snoostorm](https://www.npmjs.com/package/snoostorm) as a way to easily access the stream of Reddit comments through Snoowrap.

<pre>
  <code class="bash">
$ npm i -S snoowrap snoostorm
  </code>
</pre>

# Building out the app

Let's create a couple of new files, starting off with a blank `app.js`. This will house all of the JavaScript logic for the bot.

Next, let's make `.env`, to store those pesky environment variables. The contents of `.env` should be as follows, replacing the `***` with the values for your app:

```
CLIENT_ID=***
CLIENT_SECRET=***
REDDIT_USER=***
REDDIT_PASS=***
```

Back in `app.js`, let's start getting the bot ready. First, we need to config the environment variables with `dotenv`, and then import `Snoowrap` and `Snoostorm`

#### app.js
<pre>
  <code class="javascript">
require('dotenv').config();

const Snoowrap = require('snoowrap');
const Snoostorm = require('snoostorm');
  </code>
</pre>

Great, now let's configure the Snoowrap and Snoostorm clients, so we can start making calls to the Reddit API.

<pre>
  <code class="javascript">
// Build Snoowrap and Snoostorm clients
const r = new Snoowrap({
    userAgent: 'reddit-bot-example-node',
    clientId: process.env.CLIENT_ID,
    clientSecret: process.env.CLIENT_SECRET,
    username: process.env.REDDIT_USER,
    password: process.env.REDDIT_PASS
});
const client = new Snoostorm(r);
  </code>
</pre>

So far, this is all just boilerplate code to instantiate the Reddit clients, using the environment variables we put in `.env`. The `userAgent` value we are passing in to `Snoowrap` uniquely identifies our client, and you can read more about that [here](https://github.com/reddit/reddit/wiki/API).

Before we start getting Reddit comments, we need to configure some options that we'll pass into `Snoostorm`. This will determine the subreddit we will be streaming from, and the number of results per polling cycle. You can find the usage guide for more options [here](https://github.com/MayorMonty/Snoostorm).

<pre>
  <code class="javascript">
// Configure options for stream: subreddit & results per query
const streamOpts = {
    subreddit: 'all',
    results: 25
};
  </code>
</pre>

Finally, let's use `Snoostorm` to create a stream of comments, and listen for any new comments that come our way! For now, let's just log any comments that get posted.

<pre>
  <code class="javascript">
// Create a Snoostorm CommentStream with the specified options
const comments = client.CommentStream(streamOpts);

// On comment, perform whatever logic you want to do
comments.on('comment', (comment) => {
    console.log(comment);
});
  </code>
</pre>

By running the app with `node app`, we can do a quick test to make sure everything is working. If all is well, your app should be printing out Reddit comments as it receives them.

The fun part in making a Reddit bot is deciding what it is actually going to do, and for the most part, I'll leave that up to you. For this example, let's do something really simple. Whenever a comment is posted with the body `:(`, let's reply with a comment containing `:)` to cheer that user up.

To do this, just replace the `console.log` statement with the following:

<pre>
  <code class="javascript">
if (comment.body === ':(') {
    comment.reply(':)');
}
  </code>
</pre>

That's it! Now, the `reply` method in Snoowrap returns a promise, so you could get fancy and do some error handling in case something goes wrong. Read more about that [here](https://not-an-aardvark.github.io/snoowrap/Comment.html#reply)

## Testing the app

Now it's time to test! You can do this by temporarily changing the `subreddit` value in `streamOpts` to "testingground4bots", and restarting the app.

Head on over to [/r/testingground4bots](https://www.reddit.com/r/testingground4bots/), and post a comment that says ":(". Once you refresh the page, you should see a fresh comment reply from the bot.

Don't forget to change `subreddit` back to `all`, or whatever subreddit you want to stream from.

## Wrapping up

That wasn't so hard, was it? Setting up a Reddit bot in Node.js turns out to be fairly trivial, so if you've got a neat idea for something you want to build, now you've got the know how!

As always, if you have questions, check out the [repository](https://github.com/SyntonicApps/reddit-bot-example-node) I've created for this tutorial, or leave a comment.

Thanks for following along!
