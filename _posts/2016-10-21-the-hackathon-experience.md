---
layout: post
title: The Hackathon Experience
author: Andrew Brooke
---

A couple of weeks ago, Caleb and I had the experience of participating in our first hackathon, Revolution UC, hosted by the University of Cincinnati's chapter of ACM. 

Along with two of our friends, we came up with a fun idea for a project, a Meme Stock Market. Basically, you would be able to log in and buy and sell shares of various memes, all in realtime. When thinking about what our stack would be, we had to choose carefully (especially since the whole thing needed to be finished in 24 hours).

Since we envisioned this as a web app, AngularJS was an easy choice for the front-end. We knew it would be simple enough to implement quickly, and Angular Material handled the prettyness of our UI with (relative) ease. For the server-side components, the obvious choice was NodeJS. Both of us have had quite a bit of experience with Node, and it lets you bootstrap a web app in the drop of a hat. The tough decision came when it was time to decide on a database. We landed on Firebase (which may have been a mistake) because of the realtime capabilities, and the ability to implement user authentication through Google with just a few lines of code.

When it was time to start programming, the hardest part was dividing the work amongst ourselves. There were times when people didn't really know what they should be doing, or they ran into issues configuring the project on their machine, etc. It was a bit of a mess. I think the takeaway from this is that it's alright to plan a bit more from the organizational standpoint beforehand, so you don't run into as many hangups during the event.

After we actually got going, it was a pretty exciting experience. Since each of us worked off our own git branch, the tree view looked pretty neat.

![Tree View]({{ site.url }}/img/tree.PNG)

The non-coding aspects of the hackathon were arguably the best part. Tons of free food, coffee, t-shirts, you name it. We also got the opportunity to socialize with some of the other teams, and see what kind of cool stuff they were building.

Eventually, our two friends went home to sleep (lame), so it was just Caleb and I left to code through the night. We didn't sleep. Eventually, we got into a pretty nice rhythm for development, and our project was starting to look more complete. After working out as many bugs as we could, we started preparing our brief presentation and headed down to the awards ceremony. 

Sadly, we didn't recieve any awards for our brilliant idea, but we did get voted as one of the most popular projects, so that's a plus. Overall it was a really fun way to spend 24 hours, and I'll definitely do it again.

Feel free to check out our project on [Devpost.](https://devpost.com/software/meme-market)