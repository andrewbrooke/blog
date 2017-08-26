---
layout: post
title: slackicons - Open Source Explorers 1
author: Andrew Brooke
comments: true
github_repo: SyntonicApps/slackicons
---

Recently I had the need to generate randomized images for users on the fly, and I decided to roll my own solution from scratch. I could have used something like [identicons](https://www.npmjs.com/package/identicon) - like what Github uses - but I thought it might be more interesting to create some images that mirror Slack's default avatars.

![like this]({{ site.url }}/img/slack.png)

Here's an example of how to generate one of these icons:

#### example.js
<pre>
  <code class="javascript">
const fs = require('fs');
const slackicons = require('slackicons');

const options = {
    seed: 'slackicons',
    size: 500
};

// Creates a 500x500 .png
slackicons.generate(options).then((buffer) => {
    fs.writeFileSync('./output.png', buffer);
}).catch((err) => {
    console.error(`Error: ${err}`);
});
  </code>
</pre>

And this is what the generated image looks like:

![like that]({{ site.url }}/img/slackicons.png)

The package is available from npm with the following command, or on [Github](https://github.com/SyntonicApps/slackicons)

`npm install slackicons`
