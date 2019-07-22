Solid applications are somewhat like multi-user applications where instances talk to each other through a shared filesystem, and the Web is that filesystem.

# Make a Solid App on Your Lunch Break 

This is a quick tutorial for new Solid developers. It is so quick that you can do this on your lunch break. In a couple of simple steps, we will build a profile viewer.

## Learning objectives:

* building a basic app with Solid
* logging in and out
* reading data from a Solid pod

## Prerequisites:

* basic HTML and CSS knowledge
* intermediate JavaScript skills

## Required tools:

* your favorite text editor
* a Web server to run your app locally (for example `npm install -g local-web-server`)

### Step 0: Get yourself a Solid pod 

In order to read and write data, you will need your own Pod and identity. [Get a Pod](https://github.com/solid/pods/blob/master/README.md) if you haven't done so already.

### Step 1: Set up a basic HTML page

Create an empty HTML document that will hold your application:

<!doctype html>
<html>
<head>
  <meta charset="utf-8">
  <title>Profile Viewer</title>
</head>
<body>
  <h1>Profile viewer</h1>
</body>
</html>

Fire up your local Web server (for example `ws`) and view your page in the browser. Nice! 

[Click here](https://github.com/solid/profile-viewer-tutorial/tree/tutorials/lunch-break/steps/01) for the code for this step.


### Step 2: Add jQuery

This is just a lunch break app, so we will simply use jQuery instead of one of the more advanced frameworks. This will allow us to focus on Solid itself. We're rapidly building out support for frameworks like Angular, React, and Vue too!

[Click here](https://github.com/solid/profile-viewer-tutorial/tree/tutorials/lunch-break/steps/02) for the code for this step.

### Step 3: Add login status UI elements

Add two <p> elements to the HTML page: one that says the user is logged in, one that says they are not:

<p id="login">
  You are not logged in.
</p>
<p id="logout">
  You are logged in as <span id="user"></span>.
</p>
Use jQuery to hide the “logged in” message, since we do not have any actual authentication code yet.

[Click here](https://github.com/solid/profile-viewer-tutorial/tree/tutorials/lunch-break/steps/03) for the code for this step.


### Step 4: Add the Solid auth client
The solid-auth-client library helps us with authenticating the user and securely fetching files from their pod. You need the following components:

a script file with authentication functionality: https://solid.github.io/solid-auth-client/dist/solid-auth-client.bundle.js
a user interface for logging in: https://solid.github.io/solid-auth-client/dist/popup.html
Place the library before the main script, as we will be using its functionality there.

[Click here](https://github.com/solid/profile-viewer-tutorial/tree/tutorials/lunch-break/steps/04) for the code for this step.

### Step 5: Add a login button

Add a login button to the page, and set up an event handler such that clicking that button triggers a Solid login window. This happens through the popupLogin function provided by window.solid.auth that is initialized by solid-auth-client. We pass the location of our popup window as a parameter.

const popupUri = 'popup.html';
$('#login button').click(() => solid.auth.popupLogin({ popupUri }));
We now want the interface to update when the user is logged in. solid.auth provides the trackSession function which will invoke your callback any time the user’s login status changes. It will pass an object with a webId property when the user is logged in, or null when they are not.

solid.auth.trackSession(session => {
  const loggedIn = !!session;
  $('#login').toggle(!loggedIn);
  $('#logout').toggle(loggedIn);
  $('#user').text(session && session.webId);
});
Test your application by logging in with your pod. (Remember to view the application through a local Web server, not the filesystem.)

[Click here](https://github.com/solid/profile-viewer-tutorial/tree/tutorials/lunch-break/steps/05) for the code for this step.

### Step 6: Add a logout button

In a similar way, provide the user with logout functionality:

$('#logout button').click(() => solid.auth.logout());
Test your application by logging in and out.

[Click here](https://github.com/solid/profile-viewer-tutorial/tree/tutorials/lunch-break/steps/06) for the code for this step.

### Step 7: Add an input element for the profile’s WebID

This profile viewer will be able to show data from different people’s profiles. Add an input element so people can select that profile:

<p>
  <label for="profile">Profile:</label>
  <input id="profile">
</p>
To simplify testing, you can make the profile default to the logged in user:

solid.auth.trackSession(session => {
  // …
  if (session) {
    $('#user').text(session.webId);
    if (!$('#profile').val())
      $('#profile').val(session.webId);
  }
});

[Click here](https://github.com/solid/profile-viewer-tutorial/tree/tutorials/lunch-break/steps/07) for the code for this step.

### Step 8: Add RDFlib.js

RDFlib.js is a JavaScript library that allows us to interact with Linked Data stored in Solid pods. You can obtain it from https://linkeddata.github.io/rdflib.js/dist/rdflib.min.js.

Be sure to place this script tag after the one for solid-auth-client, such that RDFlib can use the authenticated fetch functionality.

[Click here](https://github.com/solid/profile-viewer-tutorial/tree/tutorials/lunch-break/steps/08) for the code for this step.

### Step 9: Show the user’s name

solid-auth-client allows us to perform authenticated retrieval of data from pods, and RDFlib lets us parse and process that data. We will now fetch the user’s profile document, and look for a triple that indicates the name of the person. This is possible with the following code:

const FOAF = $rdf.Namespace('http://xmlns.com/foaf/0.1/');
$('#view').click(async function loadProfile() {
  // Set up a local data store and associated data fetcher
  const store = $rdf.graph();
  const fetcher = new $rdf.Fetcher(store);

  // Load the person's data into the store
  const person = $('#profile').val();
  await fetcher.load(person);

  // Display their details
  const fullName = store.any($rdf.sym(person), FOAF('name'));
  $('#fullName').text(fullName && fullName.value);
});

[Click here](https://github.com/solid/profile-viewer-tutorial/tree/tutorials/lunch-break/steps/09) for the code for this step.

### Step 10: Show the user’s friends

Now that we can display a single person, we can also fetch their friends and show them:

$('#view').click(async () => {
  // …
  const person = $('#profile').val();
  // …
  const friends = store.each($rdf.sym(person), FOAF('knows'));
  $('#friends').empty();
  friends.forEach(async (friend) => {
    await fetcher.load(friend);
    const fullName = store.any(friend, FOAF('name'));
    $('#friends').append($('<li>')
       .text(fullName && fullName.value || friend.value));
  });
});
Note that the name of those friends is loaded from _their_ pods, not yours! In case your example account does not have friends yet, you can test with this profile in the viewer input box: https://ruben.verborgh.org/profile/#me.

[Click here](https://github.com/solid/profile-viewer-tutorial/tree/tutorials/lunch-break/steps/10) for the code for this step.

### Step 11: Make the friends clickable

With a simple step, we can make the profile viewer more interactive. Instead of just listing the friends, we turn them into links. When a link is clicked, that person is loaded into the profile viewer:

$('#view').click(async function loadProfile() {
  // …
  const person = $('#profile').val();
  // …
  const friends = store.each($rdf.sym(person), FOAF('knows'));
  $('#friends').empty();
  friends.forEach(async (friend) => {
	await fetcher.load(friend);
	const fullName = store.any(friend, FOAF('name'));
	$('#friends').append(
  	$('<li>').append(
    	$('<a>').text(fullName && fullName.value || friend.value)
            	.click(() => $('#profile').val(friend.value))
            	.click(loadProfile)));
  });
});
This simple change shows how, thanks to the power of Linked Data, we can easily traverse data stored across different data pods.

[Click here](https://github.com/solid/profile-viewer-tutorial/tree/tutorials/lunch-break/steps/11) for the code for this step.

# Solid App Tutorials 

* [Learn Solid](https://github.com/learnsolid/learnsolid.github.io)
* [Intro to Solid Tutorial](https://github.com/solid/solid-tutorial-intro)
* [Solid Apps with Angular](https://solid.inrupt.com/docs/writing-solid-apps-with-angular)
* [Solid Apps with React](https://solid.inrupt.com/docs/writing-solid-apps-with-react)
* [Jackson Morgan Instructions](https://github.com/jaxoncreed/solid-in-your-lunch-hour)
* [Developer Tools](https://github.com/solid/information/blob/master/developer-tools.md)
* [Solid Servers Tools and Packages](https://github.com/solid/solid-platform)
* [Intro to Solid](https://github.com/solid/intro-to-solid-slides)
* [Solid Tutorial Intro](https://github.com/solid/solid-tutorial-intro)
* [Solid Tutorial Angular](https://github.com/solid/solid-tutorial-angular)
* [Solid Tutorial rdflib](https://github.com/solid/solid-tutorial-rdflib.js)
* [Solid Tutorial Profile Viewer Tutorial](https://github.com/solid/profile-viewer-tutorial)
* [Solid Tutorial Understanding Linked Data](https://github.com/solid/understanding-linked-data)
* [Solid Tutorial Pastebin](https://github.com/solid/solid-tutorial-pastebin)
* [Solid Vocabularies](https://github.com/solid/vocab)
* [Releases](https://github.com/solid/releases)
* [Userguide](https://github.com/solid/userguide)
* [Solid Releases](https://github.com/solid/releases)
* [Solid Namespace](https://github.com/solid/solid-namespace) 
* https://www.w3.org/DesignIssues/diagrams/solid/2018-soild-work.svg 
* https://github.com/LearnSolid/solid-roadmap
* Pieter Heyvaert's detailed [writeup](https://forum.solidproject.org/t/decentralized-chess-game/365/20) on building [solid-chess](https://github.com/pheyvaer/solid-chess) provides a peek into architecting Solid applications.
