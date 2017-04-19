---
layout: post
title: Syntonic Tech Talk Episode 5 - Firebase Cloud Functions
author: Andrew Brooke
comments: true
---

It's been a while, but the fifth episode of our podcast is [available now!](https://www.youtube.com/watch?v=_fz9eCTE--g)

Feel free to follow along with our Firebase Cloud Functions code demo here: https://github.com/SyntonicApps/firebase-cloud-func-example

## Intro to Firebase Cloud Functions

Cloud Functions are an exciting new addition to Google's Firebase platform. Essentially, they allow you to configure triggers for a number of different types of events, and run Javascript code to respond to these events, without managing your own server.

In this episode of our podcast, we show you how to set up some Cloud Functions, but I'll run you through it here as well.

*Prerequisites: A Firebase project, npm*

### Setting up the Firebase CLI

With npm, install firebase-tools: `npm i -g firebase-tools`

Login to your account with the Firebase CLI: `firebase login`

### Creating a Firebase functions project

```
mkdir my-project && cd my-project
firebase init # configure with your options in the interactive CLI
```

That just created a lot of files. For now, we'll be paying attention to the stuff in the `functions` directory.

This is set up like a normal npm package. You can add dependencies to the `package.json`, but for now let's take a look at creating some functions in `index.js`.

### Example Functions

For our examples, we'll be creating a couple of simple functions. First, a very basic hello world that responds to an https request at the function URL.

```
const functions = require('firebase-functions');

exports.hello = functions.https.onRequest((request, response) => {
    response.send('Hello from firebase-functions!');
});
```

Simple enough. Run `firebase deploy` to get this up and running. Visit the URL output in the terminal to view the hello world message.

Next, let's try something a bit more adventurous. Back in [Podcast #3](https://www.youtube.com/watch?v=w9sg086jMe8) we talked about Geofire, a way to query location data with Firebase. Check out that episode if you haven't already seen it!

In this example, we're going to automatically create a Geofire entry for our custom object's location property, when it is written to the Firebase realtime database.

In the `functions` directory, run `npm i -S firebase geofire` to install Geofire as a dependency, and add the following to your `index.js`:

```
const GeoFire = require('geofire');

/**
 * Cloud function to write a Geofire entry for an object's location, when it is written to the database
 */
exports.geofire = functions.database.ref('/objects/{pushId}/location').onWrite(event => {
    // When data is written to '/objects/{pushId}/location' in our DB, this is called

    const data = event.data;

    // Get a reference to where the Geofire locations will be stored
    const locationsRef = data.ref.root.child('locations');
    const geoFire = new GeoFire(locationsRef);

    const parentKey = data.ref.parent.key; // Our {pushId}

    console.log(`Setting GeoFire for key: ${parentKey} to location: ${data.val()}`);

    return geoFire.set(parentKey, data.val());
});
```

Test this out by running `firebase deploy`, and adding an object like this to your Firebase database:

![Object]({{ site.url }}/img/FirebaseCloudFunctions/object.PNG)

Shortly, you should see the Geofire entry appear:

![Location]({{ site.url }}/img/FirebaseCloudFunctions/location.PNG)

If something goes wrong, you can check out the `Functions` section in Firebase to see some helpful logging information.

To learn more about Firebase Cloud Functions, check out [Google's documentation](https://firebase.google.com/docs/functions/).
