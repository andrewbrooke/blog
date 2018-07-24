---
layout: post
title: Programming a Discord Bot with NodeJS
author: Andrew Brooke
comments: true
github_repo: SyntonicApps/discord-reverser-bot
---

If you're unfamiliar with [Discord](https://discordapp.com/), it's a text & voice chat application that targets gamers. You can create your own servers, channels within those servers, permissions, etc. And best of all, you can hack around on their [API](https://discordapp.com/developers/docs/intro) to make your own bot to say, tell you the weather, or show you pictures of cats.

In this tutorial, we will be using NodeJS and discord.js to write a simple bot for Discord.

# Creating a Discord app

So, what exactly is a Discord Bot? Well, it's basically a robot user that you add to a server and bend to do your will. You could use it for all sorts of things, but in this tutorial we'll be using it to reverse whatever message the user sends to it. Riveting stuff.

First things first, you'll need to head on over to [My Applications](https://discordapp.com/developers/applications/me) on the Discord site (create an account if you don't already have one).

Go ahead an click on the "New Application" button to create a new Discord app.

![New App]({{ site.url }}/img/Discord/new_app.PNG)

Now, give your app a name and optionally a description and icon. Our app is going to simply be called "reverser-bot", but feel free to pick anything you like. Then, hit "Create Application."

On the next page, you need need to click on the "Create a Bot User" button. This is what is going to actually be interacting with the users in your Discord server.

![Create a Bot User]({{ site.url }}/img/Discord/bot.PNG)

NOTE: You'll need both your app's client ID and this bot's Token later on in the tutorial, so make note of them now

# Setting up the NodeJS project

If you don't already have it, go ahead and install [NodeJS](https://nodejs.org).

NOTE: NodeJS 6.0.0 or higher is required for use of discord.js, a library we will be using in this tutorial.

Fire up the command line and enter the following

<pre>
  <code class="bash">
$ mkdir discord-bot
$ cd discord-bot
$ npm init
  </code>
</pre>

Here, you're going to enter the information from which npm will configure the `package.json` for your project. This file will contain some metadata about your project, as well as keep track of any dependencies we may need.

You can customize this as much as you want, but if you just press `return` a few times you'll be good to go.

For this project, we will be using [discord.js](https://github.com/hydrabolt/discord.js/), a library that will allow us to interact with the Discord API directly from NodeJS.

To install this dependency to your project (and save it in `package.json`), run the following command

<pre>
  <code class="bash">
$ npm install --save discord.js --production --no-optional
  </code>
</pre>

NOTE: This build does not include support for voice connections, the following command will install the dependency with support for voice (`npm install --save discord.js --production`)

# Coding the Bot

Open up the project directory in your favorite text editor, and create a new file at the root level: `index.js`. This is where we're going to stick all the code for Mr. Bot.

First off, let's import the discord.js module and configure the application that we created earlier.

#### index.js
<pre>
  <code class="javascript">
// Import the discord.js module
const Discord = require('discord.js')

// Create an instance of Discord that we will use to control the bot
const bot = new Discord.Client();

// Token for your bot, located in the Discord application console - https://discordapp.com/developers/applications/me/
const token = 'YOUR-TOKEN-HERE'
  </code>
</pre>

The next thing we need to do is configure some events for the bot. When we send a message containing a certain keyboard to the server, we need the bot to respond. For simplicity, let's make it so if the message begins with `!`, our bot will respond to the user.

So, a message to the bot might look like this: `!Hello there bot`

Everything after the exclamation point is what we want to reverse and send back, so the response would look like this: `tob ereht olleH`

#### index.js
<pre>
  <code class="javascript">
// Gets called when our bot is successfully logged in and connected
bot.on('ready', () => {
    console.log('Hello World!');
});

// Event to listen to messages sent to the server where the bot is located
bot.on('message', message => {
    // So the bot doesn't reply to iteself
    if (message.author.bot) return;

    // Check if the message starts with the `!` trigger
    if (message.content.indexOf('!') === 0) {
        // Get the user's message excluding the `!`
        var text = message.content.substring(1);

        // Reverse the message
        var reversed = '';
        var i = text.length;

        while (i > 0) {
            reversed += text.substring(i - 1, i);
            i--;
        }

        // Reply to the user's message
        message.reply(reversed);
    }
});
  </code>
</pre>

Thats's it for the meat of the code. The only thing remaining is to tell our bot to login.

<pre>
  <code class="javascript">
bot.login(token);
  </code>
</pre>

# Testing the Bot

Remember that client ID I mentioned earlier? We're going to need it now. To add the bot to your server, visit the following URL, where APP_ID is replaced by your Client ID.

[https://discordapp.com/oauth2/authorize?client_id=APP_ID&scope=bot](https://discordapp.com/oauth2/authorize?client_id=APP_ID&scope=bot)

You'll be asked to select a server for the bot to join, so create one on Discord for testing if you need to. (It's free!)

![Authorizing the Bot]({{ site.url }}/img/Discord/auth.PNG)

Once you pick a server, you can open up the Discord client and see that your bot has been added to the server!

![It's in]({{ site.url }}/img/Discord/users.PNG)

There's just one last step before we can use our bot, we need to run our Node project. To do this, simply return to the command line and run the following command

<pre>
  <code class="bash">
$ node index
  </code>
</pre>

If all is well and good, you should see a message appear in your console, and the bot will become active on Discord! Now, let's try out the functionality.

Send a message beginning with `!` in the server, and see what happens

![Chat]({{ site.url }}/img/Discord/chat.PNG)

Success!

# Final Thoughts

That does it for this tutorial, I hope you enjoyed following along. Bots are a nice way to spice up any Discord server, and can be expanded to do pretty much anything you can think of.

We've posted the full code for this tutorial on Github, so feel free to [take a look](https://github.com/SyntonicApps/discord-reverser-bot)
