Solid applications are somewhat like multi-user applications where instances talk to each other through a shared filesystem, and the Web is that filesystem.

## Table of Contents

1. [Make a Solid App on Your Lunch Break](#make-a-solid-app-on-your-lunch-break)
2. [Writing Solid Applications with Angular](#writing-solid-applications-with-angular)
3. [Writing Solid Applications with React](#writing-solid-applications-with-react)
4. [More Solid App Tutorials](#more-solid-app-tutorials)

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

# WRITING SOLID APPLICATIONS WITH ANGULAR

GETTING STARTED

The easiest way to get started developing Solid with Angular is to use the solid-angular Yeoman generator. If you’re new to Yeoman, you can check out the helpful Yeoman Getting Started guide. At its core, Yeoman is a scaffolding tool that will install all the basic files, folders, and dependencies that you will need to start coding right away.

The code for the solid-angular generator can be found on github: https://github.com/inrupt/generator-solid-angular. 

In a command line window, follow these steps:

npm install -g install @inrupt/generator-solid-angular 
Navigate to the root project folder you want the app to live in
yo @inrupt/solid-angular
Set an application name / folder name
Angular files and dependencies are installed with a sample application ready to go
Once these steps are complete, you will have a sample application showing the basics of a Solid app. It will be able to login users in via Solid, and authenticate, fetch data from a pod, and update or delete data from a pod. You can start the application using angular-cli as usual, simply by using ng serve.

Welcome to Solid.

DEPENDENCIES

Below is a high-level list of dependencies for this app. It is not a comprehensive list by any means, but hits the major dependencies and gives a brief description of what they are.

Angular 6 and standard relevant libraries
This should be self-explanatory! You can add more angular libraries if you need to, but the core Angular libraries are required for this sample application.
Github: https://github.com/angular/angular
Website: https://angular.io/
rdflib.js
See documentation above on using rdflib.js. This is a required library for the sample application and will be the primary interface with the linked data graph used pervasively in Solid pods.
Github: https://github.com/linkeddata/rdflib.js
Website: http://linkeddata.github.io/rdflib.js/doc/
solid-auth-client
See documentation above on using solid-auth-client and how authentication works for Solid. This is a required library for the sample application and will authenticate the user.
Github: https://github.com/solid/solid-auth-client
Website: https://solid.github.io/solid-auth-client/
PureCSS
This is an optional library. Out of the box, the sample application uses the pureCSS grid system for layout purposes but it can be replaced by any layout system, or a custom layout solution.
Github: https://github.com/pure-css/pure/
Website: https://purecss.io/
ngx-toastr
This is an optional library. For messaging purposes user-facing alerts and notifications, the sample application uses the ngx-toastr library. This can be replaced by any toast, alert, or popup system you choose and is intended only for demonstration purposes.  
Github: https://github.com/scttcper/ngx-toastr
Website: https://scttcper.github.io/ngx-toastr/ 
WHAT THE SAMPLE APPLICATION DOES

This sample application is a basic profile viewing and editing application. It consists of only a few pages and routes. There’s a login page and a profile page. 

You can login to see how authentication works, and view your profile information. Once it’s loaded, you can save or delete data using the profile form as well.

The goal of the sample app is to show the authentication process, as well as data manipulation inside of Solid, and provide an example of how to do it within the angular ecosystem.

CODE WORKFLOW

Step 1: Registration

The first thing your users will need is a pod on a Solid server. You can get a pod quickly from an existing provider (link to solid site / providers page), or setup one on your own solid server (link to server setup doc).  

To get started right away, you can register with an existing Solid pod here.

In the future, we plan to ship an example registration workflow with the Solid-angular generator.

Step 2: Login

Once registered with a pod, your user can login using that provider. Provided in the sample application is a functional login workflow. By default, the page will redirect to the provider’s login page, then return to a url provided in the login call. In our example, it returns us to the profile page.

An alternate workflow is also available if you don’t want to fully redirect your application. There is also a login popup, which will open a login prompt in a popup window instead. 

Once login is complete, a localStorage item is created with the user’s token.

In our current angular app example, we provide route guards against this localStorage object being missing. Another example of how angular could handle an unauthorized user is to use an Interceptor for 401 responses and redirect to login.

Step 3: Load Profile

One the user is authenticated, the /card route will load. This is a profile card, and is intended only as an example of how to work with RDF data. The profile card page does a few things. First, it tries to getch the data using the rdf.service.ts angular service. Next, it takes the old form data and stashes it in localstorage.

This stashing of data is important, as we need a cached version of the original form data for the purposes of updating (more on that later).

The rdfService call “getProfile” first uses the fetcher to load the current logged in user’s webID, then plucks the profile values out one at a time and maps them to a return object.

await this.fetcher.load(this.session.webId);

return {
    fn : this.getValueFromVcard('fn'),
    company : this.getValueFromVcard('organization-name'),
    phone: this.getPhone(),
    role: this.getValueFromVcard('role'),
    image: this.getValueFromVcard('hasPhoto'),
    address: this.getAddress(),
    email: this.getEmail(),
};
The calls getValueFromVcard() and getPhone() etc, are helper functions in the rdfService. Here’s an example of what these functions look like:

getValueFromVcard = (node: string, webId?: string) => {
   const store = this.store.any($rdf.sym(webId || this.session.webId), VCARD(node));
   if (store) {
       return store.value;
   }
   return '';
}
As you can see, it looks through the store for any value of a Vcard node that’s supplied, then returns the value (or an empty string). This is just a helper to simplify the process, since we used a lot of Vcard fields in this example we didn’t feel like we wanted to re-write the any() call multiple times, when we could just pass in a node name. 

Once the data is loaded, getProfile() returns an object containing the profile fields. This is a custom object we created. Once that object is back in the card page, we bind that to the UI.

For the purposes of this demo, we used the form input “name” field to map to the nodeName for easy mapping. Our goal is to have a more built-in way to do this, but for now we’re relying on manual data mapping.

Step 4: Save / Update Profile

Once the profile is loaded, the form will display on the card page. If the user changes any field, and that angular form becomes dirty/touched, then a Save button will become available. 

The save function has some things in it that are non-standard for current web development, so I’ll walk through that code here.

The card page code is pretty simple. On form submit, we simply called the angular rdfService’s “updateProfile” function and pass the form in. On success, the card page saves the newly saved values in localstorage as the “cached” version.

In the rdfService, the updateProfile(form) call does a lot. First, it sets up some variables used in the rdf update call. These include the logged in webID and profile links. Next, it calls a long function called transformDataForm. This call takes a few parameters that we set up in this function.

The main purpose is to provide an output object containing an array of insertions and an array of deletions. Any new value will be in the insertions array and any changed or removed value will be in the deletions array. We need to map the form to these two arrays. If a field was changed, that would mean we need both a deletion item and an insertion item (it expects us to delete the old value and insert a new one in the same node).

To do this, we check the form field status, and only process dirty fields. No sense in updating unchanged fields. Next, we make sure the field exists. If not, it’s an insertion, and we add the data that’s expected of an insert.

Both the insertion and deletion arrays expect the same thing: an rdf statement. The ”statement” consists of the URI for the field, which in most cases is just the #me profile link. Next, it expects the node information - in this case “VCARD(fieldName)”. Third, it expects the value to either save or delete. Lastly, it expects the link to where the data is stored, in this case your webID without the #me at the end.

Once all that processing is complete, the updateProfile() call continues. The actual call to save is here, and is uses something called the updateManager. 

this.updateManager.update(data.deletions, data.insertions, (response, success, message) => {
   //processing code 
}
As you can see, we pass in the deletions and insertions straight to the updateManager call. That will process and save the data in the arrays, and if it returns a success, we show a toast notification and reset our form to pristine and untouched.

CODE STRUCTURE

The code structure should be very familiar to angular developers. For the most part, it maintains the structure of an out-of-the-box angular-cli generated application. Inside of the app folder, we created a few example components and filled them in where applicable.

We purposely left the code as simple as possible, as the focus should be learning how to work with Solid and rdflib.js. Feel free to add new services, abstracts, interfaces, and so on, but we chose not to use these in order to streamline the Solid code as much as possible.

Areas of Interest

Here are the most relevant files to learn more about angular Solid development.

src/app/card/card.component
This is the main profile page
src/app/home/home.component
This is our login page
src/app/login/login.component
This is our login popup component - currently unused, but the code exists to swap between inline redirect or popup redirect methodologies.
src/app/services/rfd.service.ts
The main interface between the angular app and rdflib
NOTES

A few quick notes about the code, as there are some hidden things you should know.

As mentioned above, the form is currently manually mapped to field names via the name attribute. There are other ways to do this, but this is a simple way to get started. In the future we hope to have a more automated way of mapping forms to data.
Our form doesn’t handle fields with multiple values. For example you can have different phone numbers with different “types”. The app currently just grabs the first value and uses that. Same with email. If you look at the getPhone() or getEmail() functions, they perform a string split() on the separators, and grab index 1, which will be the first actual phone number.
Address is currently not working. This is because address is not a field but a set of fields, and the UI/UX assumed it was a single field. We left this in as a failing field to show how to handle errors.
Clicking the profile image at the top right of the menu bar will log you out.
Some of our dependencies have their own dependencies that needed tweaking out of the box for angular 6. To fix any dependency errors, we had to add to tsconfig.ts some exceptions and paths.

# WRITING SOLID APPLICATIONS WITH REACT

GETTING STARTED

The easiest way to get started developing Solid with React is to use the Solid-React Yeoman Generator.

Prerequisites

Administrator Privilege

You will need administrative privilege on your local computer to install the Generator and prerequisites. Depending upon your operating system:

Mac:
Prefix the command lines to be executed with sudo.
You will be prompted for the Administrator password. Refer to: https://support.apple.com/en-us/HT202035.
Windows:
Type cmd in the search bar.
Right click on "Command prompt" and select "Run as Administrator".
npm and Yeoman

To install the Generator, you will need both npm and Yeoman if you don't already have them.

npm makes it easy for JavaScript developers to share and reuse code, and makes it easy to update the code that you’re sharing. To install, follow the instructions at npm.
Yeoman provides a generator ecosystem to scaffold complete projects, installing all the basic files, folders, and dependencies that you will need to start coding right away. If you’re new to Yeoman, you can check out the helpful Yeoman Getting Started guide. You can install Yeoman using the following command: 
npm install -g yo
Installation

The code for the Solid-React Generator can be found on github: https://github.com/inrupt/generator-solid-react. To install the Generator, in a command line window follow these steps:

Execute:

npm install -g @inrupt/generator-solid-react

Usage

Once the Generator is installed, you can create a new application with just a few steps:

Navigate to the root project folder you want the application to live in.

Execute:

yo @inrupt/solid-react

You will prompted to set:

An application / folder name.

A version number.

Whether the application is private or public.

React libraries and dependencies are then installed, together with a sample application.

Note: We have noticed an error is sometimes thrown when the Generator tries to install one of the dependencies of application. If this occurs, try installing Git and then rebuilding your application.

You can start the sample application by navigating to the newly created directory and executing:

npm run start
Welcome to Solid!

Dependencies

Below is a high-level list of dependencies for this application. It is not a comprehensive list by any means, but hits the major dependencies and gives a brief description of what they are.

React:

This should be self-explanatory.

Github: https://github.com/facebook/react

Website: https://reactjs.org/

LDFlex:

A querying library for linked data.

Github: https://github.com/solid/query-ldflex

Website: https://solid.github.io/ldflex-playground/

Solid React Components:

A set of common components for Solid applications.

Github: https://github.com/inrupt/solid-react-components

Solid Style Guide:

Styles and classes to provide a suggested look and feel to Solid applications.

Github: https://github.com/inrupt/inrupt-atomic-styleguide

Website: https://design.inrupt.com

WHAT THE SAMPLE APPLICATION DOES

This sample application is a basic profile viewing and editing application. It consists of only a few pages and routes. There’s a login page and a welcome page displaying basic profile information.

You can login to see how authentication works, and view your profile information. It also demonstrates some of the best practices of Solid application development, including how to use some of the provided libraries.

The goal of the sample application is to show the authentication process, data manipulation inside of Solid, and to provide an example of how to do it within the React ecosystem.

CODE WORKFLOW

Step 1: Registration

The first thing your users will need is a POD on a Solid server. We have provided a registration link and workflow in the generated application, which integrates with Solid server. Out of the box, only a few Providers are supported for now, but you can easily add more.

To get started right away, you can register for a POD with an existing Solid Provider here.

Step 2: Login

Once registered for a POD with a Solid Provider, your user can login using that Provider. The sample application includes a functional login workflow. By default, the page will redirect to the Provider’s login page, then return to a URL provided in the login call. In our example, it returns us to the welcome page.

Once login is complete, a localStorage item is created with the user’s token. In the React application example, we provide private routes against this localStorage object being missing.

Step 3: Welcome/View Profile

After the user is successfully authenticated, the /welcome route will load. This is a landing page that contains some explanation, and is intended only as an example of how to read data using the LDFlex library.

CODE STRUCTURE

The code structure should be familiar to React developers. It was generated with Create-React-App, and is built using Presentational / Container Components. This architecture allows the data logic to be separated from the presentation or UI layer.

We also make use of React Styled Components for further code cleanliness. Using Styled Components, we can make named Components to handle styling, so the markup is cleaner and easier to read.

Areas of Interest

Here are the most relevant files to learn more about React Solid development:

src/containers:

This folder is where most of the pages/components live. It contains components such as:

Login Page.

Registration Page.

Welcome Page.

src/services:

This folder contains any services required explicitly by the generated application.

Currently, this is only the Providers service, which is used to get a list of Providers. This is only temporary until a Provider registry exists.

src/utils:

This folder contains helpers for the generated application.

Examples include:

Styled Component helpers, for things like common media query sizes.

Enzyme test setup helpers.

# More Solid App Tutorials 

* [Learn Solid](https://github.com/learnsolid/learnsolid.github.io)
* [Intro to Solid Tutorial](https://github.com/solid/solid-tutorial-intro)
* [Solid Apps with Angular](https://solid.inrupt.com/docs/writing-solid-apps-with-angular)
* [Solid Apps with React](https://solid.inrupt.com/docs/writing-solid-apps-with-react)
* [Jackson Morgan Instructions](https://github.com/jaxoncreed/solid-in-your-lunch-hour)
* [Developer Tools](https://github.com/solid/information/blob/master/documentation/developer-tools.md)
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

Client development began with pure javascript apps with few or no external dependencies and javascript Angular apps such as [Warp](https://github.com/linkeddata/warp) and [Plume](https://github.com/deiu/solid-plume).  Plume now has different implementations, one of which [uses the Safe network](https://github.com/theWebalyst/solid-plume).  A client library abstracting interaction with pods, called [solid-auth-client](https://github.com/solid/solid-auth-client), was developed.  Originally client apps used a library for [RDF](https://www.w3.org/TR/rdf-primer/) serialization/deserialization and manipulation called [Rdflib.js](https://github.com/linkeddata/rdflib.js/).  A library for HTML5 user interface widgets for Solid, which uses solid-auth-client, was developed, called [solid-ui](https://github.com/solid/solid-ui) which is integrated with RDF using Rdflib.js, and it is used as the default user interface library via the [Data Browser aka Mashlib](https://github.com/linkeddata/mashlib) which is included with Solid pods.

Rdflib.js is still used for many client apps, but another library for RDF has been developed called [Comunica](https://github.com/comunica), which is a modular implementation of a [Triple Pattern Fragments](https://www.hydra-cg.com/spec/latest/triple-pattern-fragments/) client. A Triple Pattern Fragment is data of a size situated midway between a full data dump, which is highly available to a client but comes at the high cost of downloading, and individual [SPARQL](https://www.w3.org/TR/rdf-sparql-query/) queries, which have a high server cost and servers are often unavailable. Triple pattern fragments fill a need to make linked data queries more efficient.  Comunica is a modular framework, and it has an actor implementation that uses solid-auth-client available.

Both Rdflib.js and Comunica implement an emerging standard for RDF interaction called [Rdf/js](https://github.com/rdfjs), which has a community group at the W3C, and they “strive to design interface specifications with the goal that different JavaScript implementations of RDF concepts can interoperate”.  They have a specification for a data model.  Both Rdflib.js and Comunica support Rdf/js.

The [React application generator](https://github.com/inrupt/generator-solid-react) and React examples are built on [LDflex](https://github.com/RubenVerborgh/LDflex), which is built on Comunica.  Solid-panes, which is included as the default user interface for Solid pods via the Data Browser, is built on solid-ui, which is built on Rdflib.js.

# Solid Angular Tutorial

This book is a set of tutorials for building apps on the
[Solid](https://solidproject.org/) platform.

This book is **not** an introductory guide to the Solid core platform, more
designed as exercises to demonstrate what is possible. Each chapter will
contain a tutorial showing how to build certain types of apps. In general, it
is advisable to go through each chapter in order.

The audience for this book is developers familiar with web technology that wish
to learn how to use decentralized technology to create the next generation of
web applications.

So, let's get started!  Please continue to [Chapter 1](https://melvincarvalho.gitbooks.io/solid-tutorials/content/chapter1.html).

## App Gallery

### [Solid Hello World](https://melvincarvalho.gitbooks.io/solid-tutorials/content/chapter1.html)

![Solid Hello World](https://melvincarvalho.gitbooks.io/solid-tutorials/content/solidhello.png)

### [Solid Clip](https://melvincarvalho.gitbooks.io/solid-tutorials/content/chapter2.html)

![](https://melvincarvalho.gitbooks.io/solid-tutorials/content/clip.png)

### [Solid Video](https://melvincarvalho.gitbooks.io/solid-tutorials/content/chapter3.html)

![](https://melvincarvalho.gitbooks.io/solid-tutorials/content/video.png)

### [Solid Words](https://melvincarvalho.gitbooks.io/solid-tutorials/content/chapter4.html)

![](https://melvincarvalho.gitbooks.io/solid-tutorials/content/words.png)

### [Solid Chess](https://melvincarvalho.gitbooks.io/solid-tutorials/content/chapter5.html)

![](https://melvincarvalho.gitbooks.io/solid-tutorials/content/solidchess.png)

### [Solid Explorer](https://melvincarvalho.gitbooks.io/solid-tutorials/content/chapter6.html)

![](https://melvincarvalho.gitbooks.io/solid-tutorials/content/solidexplorer.png)

#### Chapter 1 - Solid Hello World

![](https://melvincarvalho.gitbooks.io/solid-tutorials/content/solidhello.png)

## Introduction

In this tutorial we will cover how to build a simple client side hello world app
using the [Solid](https://solidproject.org/) framework.

*What you will learn*

* How to create your first client side [Solid](https://github.com/solid) app
* How to use decentralized login and logout
* How to delegate HEAD requests to identify using the [User](https://www.w3.org/community/rww/wiki/User_Header) header
* How to use notifcations using [Lumx](http://ui.lumapps.com/) and [AngularJS](https://angularjs.org/)

## The App

[Solid](https://github.com/solid) apps typically run client side and do not
require a server, except to identify a user, or store data.

This app written using on [AngularJS](https://angularjs.org/). Setup and
scaffolding of the app is out of scope for this tutorial. But there are many
[guides](https://docs.angularjs.org/misc/started) online that show you how to
get started. The code is also available for download in the footnotes.

Hello world is a simple app that allows decentralized login and logout using the
[WebID](http://webid.info/) Identity system. A remote server will perform the
authentication, typically using TLS, but any authentication method is allowed
provided that the [User](https://www.w3.org/community/rww/wiki/User_Header)
header is sent back.

First, we will look at how to use decentralized login using JavaScript. Using
[WebID](http://webid.info/) it is possible to login using an HTTP URI that
denotes you as person, and that can also be dereferenced to find out more about
you. In this app, we will simply get the URI of the user and display it on
screen. The **login** code is below:

```javascript
    $http({
      method: 'HEAD',
      url: AUTHENDPOINT,
      withCredentials: true
    }).success(function(data, status, headers) {
      var header = 'User';
      var scheme = 'http';
      var user = headers(header);
      if (user && user.length > 0 && user.slice(0,scheme.length) === scheme) {
        $scope.notify('Login Successful!');
        $scope.loggedIn = true;
        $scope.user = user;
      } else {
        $scope.notify('WebID-TLS authentication failed.', 'error');
      }
      $scope.loginTLSButtonText = 'Login';
    }).error(function(data, status, headers) {
      $scope.notify('Could not connect to auth server: HTTP '+status);
      $scope.loginTLSButtonText = 'Login';
    });
```

The system used is a delegated authentication. What that means is that we use a
server to identify and verify who is using the app. This is because a server is
required in order to verify who a user is.

The `AUTHENDPOINT` in our example is set to: https://databox.me/

```javascript
    AUTHENDPOINT = "https://databox.me/";
```

The `withCredentials` flag is set to true in order to prevent a CORS error.

A HEAD request to any WebID enabled server will return a `User:` header which
specifies  who is using the app. That user can then be used to customize the
app. In our case we simply set `$scope.user`. This is enough to login to our
simple app, and display a hello world message.

The **logout** code simply unsets the `$scope.loggedIn` variable, and returns
you to the start screen:

```javascript
  $scope.logout = function() {
    $scope.init();
    $scope.notify('Logout Successful!');
  };
```

Putting this simple functionality together in using the
[AngularJS](https://angularjs.org/) framework (with
[lumx](http://ui.lumapps.com/) extensions) it is possible to create a simple
demo:

[Hello World Live Demo](http://melvincarvalho.github.io/helloworld/)

The `$scope.notify` function returns a message to the user to tell them they
have logged in or out. This completes the functionality of this simple app.

## Summary

In this chapter we have shown how to identify and verify a user using delegated
login and WebID. We have packaged the code into an
[AngularJS](https://angularjs.org/) app and presented a simple demo. In the next
chapter we will show how to save data to your Personal Online Datastore (Pod).

## See Also

* [Source Code](https://github.com/melvincarvalho/helloworld)
* [Live Demo](http://melvincarvalho.github.io/helloworld/)
* [Solid](https://github.com/solid)
* [WebID](http://webid.info/)
* [AngularJS](https://angularjs.org/)
* [Lumx](http://ui.lumapps.com/)
* [User Header](https://www.w3.org/community/rww/wiki/User_Header)

#### # Chapter 2 - Solid Clipboard

![](https://melvincarvalho.gitbooks.io/solid-tutorials/content/clip.png)

## Introduction

In this tutorial we will cover how to load and save data to a Personal Online
Datastore (Pod). The app is a simple web clipboard that allows you to save data
to a store and then recover it later.

*What you will learn*

* How to set up [rdflib.js](https://github.com/linkeddata/rdflib.js/) to work
    with the web of data
* How to read data from a Pod
* How to write data to a Pod
* How to change the location of the URL you are using
* How to change query string to allow bookmarking
* How to use the `#this` pattern
* How to use the `urn:tmp:` pattern

## The App

This app builds on the previous hello world app, which enables login and logout
by introducing Personal Data Stores (Pods) and allowing read and write to those
stores using [rdflib.js](https://github.com/linkeddata/rdflib.js/). The app
operates as a simple web clipboard that lets you save text to a location and
retrieve it later.

The first thing that we will do is set up
[rdflib.js](https://github.com/linkeddata/rdflib.js/). It is sourced in to
`index.html`

```html
    <script src="vendor/rdflib.min.js"></script>
```

After sourcing in the library, the first thing to do is to **initialize** a
knowledge base graph, and a fetcher. This is done using the `$rdf` global
variables.

```javascript
    g = $rdf.graph();
    f = $rdf.fetcher(g);
```

The variable `g` will contain everything that is fetched using the fetcher, and
also meta data about the documents that have been fetched. The variable `f` can
be used to fetch documents.

For convenience some common namespaces are set up so that RDF vocabularies can
be called conveniently with a shorthand using `$rdf.Namespace`:

```javascript
  var CHAT  = $rdf.Namespace("https://ns.rww.io/chat#");
  var CURR  = $rdf.Namespace("https://w3id.org/cc#");
  var DCT   = $rdf.Namespace("http://purl.org/dc/terms/");
  var FACE  = $rdf.Namespace("https://graph.facebook.com/schema/~/");
  var FOAF  = $rdf.Namespace("http://xmlns.com/foaf/0.1/");
  var LIKE  = $rdf.Namespace("http://ontologi.es/like#");
  var LDP   = $rdf.Namespace("http://www.w3.org/ns/ldp#");
  var MBLOG = $rdf.Namespace("http://www.w3.org/ns/mblog#");
  var OWL   = $rdf.Namespace("http://www.w3.org/2002/07/owl#");
  var PIM   = $rdf.Namespace("http://www.w3.org/ns/pim/space#");
  var RDF   = $rdf.Namespace("http://www.w3.org/1999/02/22-rdf-syntax-ns#");
  var RDFS  = $rdf.Namespace("http://www.w3.org/2000/01/rdf-schema#");
  var SIOC  = $rdf.Namespace("http://rdfs.org/sioc/ns#");
  var SOLID = $rdf.Namespace("http://www.w3.org/ns/solid/app#");
  var TMP   = $rdf.Namespace("urn:tmp:");
```

Next, we want to pull in the data, so we use the `nowOrWhenFetched` function of
the fetcher to get data from a Pod.

Before we can do this we must determine the location from the query string or
from a default as follows, using the angularJS search function:

```javascript
    var storageURI = 'https://clip.databox.me/Public/.clip/Public/test';
    if ($location.search().storageURI) {
      storageURI = $location.search().storageURI;
    }
```

For this app, the document is assumed to contain data of the form

```html
    <#this> <urn:tmp:clipboard> "data" .
```

The data above uses RDF/[Turtle](http://www.w3.org/TR/turtle/) notation. The
`#this` subject is used to distinguish between document and data, and is similar
to the `this` keyword in JavaScript.

A best practice would be to mint an HTTP URI for the predicate. But for ease of
demonstration purposes, a temporary URN is used at under the scheme `urn:tmp:`.
It is advisable always to use HTTP URIs, rather than schemes such as `urn:`,
when dealing with linked data; this will be covered in the next tutorial.

Now we can fetch the data:

```javascript
    f.nowOrWhenFetched(storageURI, undefined, function(ok, body) {
      var clipboard = g.any($rdf.sym(storageURI + '#this'), TMP('clipboard'));
      $scope.clipboard = clipboard;
      $scope.storageURI = storageURI;
    });
```

The `nowOrWhenFetched` will call a callback either if the document is loaded in
the graph, or once it is fetched from the web. The `$rdf.sym` function simply
changes a string into a URI by placing angle brackets around it. We use the
function `g.any` to get the object of the namespace TMP of value `'clipboard'`.

Once we have fetched this data we can set it on the screen, and also set the
`storageURI` which has come back successfully.

To save the clipboard we issue a PUT request to the server.

```javascript
    $http({
        method: 'PUT',
        url: $scope.storageURI,
        withCredentials: true,
        headers: {
            "Content-Type": "text/turtle"
        },
        data: '<#this> <urn:tmp:clipboard> """' + clipboard + '""" .',
    }).
    success(function(data, status, headers) {
      $scope.notify('clipboard saved');
      $location.search('storageURI', $scope.storageURI);
    }).
    error(function(data, status, headers) {
      $scope.notify('could not save clipboard', 'error');
    });
```

The line

```javascript
    $location.search('storageURI', $scope.storageURI);
```

Changes the query string to the storage URI so that the clipboard can be
bookmarked or shared. It is important on the web that URIs reflect the state of
a given app. Astute observers will also note that the address bar now becomes an
input form where changing the query string can change the content.

Putting this all together it should be possible to see a demo as follows:

[Solid Clipboard Live Demo](http://melvincarvalho.github.io/clip/)

The default location for storing the clip in this demo is the public location
`https://clip.databox.me/Public/.clip/Public/test`, but in practice you would
want to store data in a private workspace under your storage root. Automatically
finding endpoints will be covered in future. See Appendix B for more details.

A typical location could be

```
    <storage>/Private/.clip/clip1
```

## Summary

In this tutorial we learned how to set up
[rdflib.js](https://github.com/linkeddata/rdflib.js/) in order to read and write
structured data to a personal data server. We looked at patterns for naming and
functions to read, write, and allow bookmarking. In the next tutorial we will
look at how to embed media in a webpage.

## See Also

* [Source Code](https://github.com/melvincarvalho/clip)
* [Live Demo](http://melvincarvalho.github.io/clip/)
* [rdflib.js](https://github.com/linkeddata/rdflib.js/)

#### Chapter 3 - Solid Video

![](https://melvincarvalho.gitbooks.io/solid-tutorials/content/video.png)

## Introduction

In this tutorial we will cover how to load and save data video links to a
Personal Online Datastore (Pod) and display it on screen. A slightly more
sophisticated UI will also be used.

*What you will learn*

* How to read and write video URLs to a Pod
* How to embed video in a page
* How to add a sidebar
* How to add a menu bar
* How to display modal dialogs
* How to add a DOAP project file

## The App

The video app follows the model of the previous clipboard app but adds a few
more features to the UI and allows embedding of a video element via an iframe. A
demo and the source code can be found in the footnotes.

The data format used for storing a video in a file here is:

```html
    <#this> <http://rdfs.org/sioc/ns#content> "content" .
```

The predicate here is an HTTP URI that uses the
[SIOC](http://rdfs.org/sioc/spec/) vocabulary. In this case, the content is an
embeddable video URL (as a string). This is fetched after login using the code:

```JavaScript
  $scope.fetchVideo = function() {
    f.nowOrWhenFetched($scope.storageURI, undefined, function(ok, body) {
      var video = g.any($rdf.sym($scope.storageURI + '#this'), SIOC('content'));
      $scope.setVideo(video);
    });
  };
```

The `setVideo` function simply embeds the video file in an `iframe` element
after determining the width and height (for mobile optimization). Note: because
of [CORS](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing), not all
video URLs are embeddable in an `iframe`. For example, when working with Youtube
videos, their URLs should take the form `/embed/<id>`.

```javascript
    var height = Math.round(( width * 3 ) / 4);
    var iframe = '<iframe width="' + width + '" height="' + height +
                 '" src="'+uri+'"></iframe>';
    $('#video').empty().append(iframe);
```

While previous apps have been single-canvas, this app adds a few more features
to allow extensibility. The CSS `flexbox` property is used to add a header and
side bar. There are many guides on using `flexbox` online, so it wont be covered
here.

```html
    <!-- menu -->
    <div class="card">
      <div class="toolbar">
        <div class="toolbar__left mr+++">
          <button class="btn btn--l btn--black btn--icon" lx-ripple>
            <i class="mdi mdi-menu"></i>
          </button>
        </div>
        <span class="toolbar__label fs-title">Videos</span>
        <div class="toolbar__right">
          <lx-dropdown position="right" from-top>
            <button class="btn btn--l btn--black btn--icon" lx-ripple lx-dropdown-toggle>
              <i class="mdi mdi-dots-vertical"></i>
            </button>
            <lx-dropdown-menu>
              <ul>
                <li><a ng-click="openDialog('about')" class="dropdown-link">About</a></li>
              </ul>
            </lx-dropdown-menu>
          </lx-dropdown>
        </div>
      </div>
    </div>
    <!-- end menu -->
```

The code snippet above shows the addition of a menu toolbar. In this case some
of the [Lumx](http://ui.lumapps.com/) effects are used, including dropdown,
ripple, toggle and menu. A dropdown is added that triggers an about model using
the `openDialog('about')` function. The code for this brings to the top the
`about` div and allows it to close on escape.

```javascript
  $scope.openDialog = function(elem) {
    LxDialogService.open(elem);
    $(document).keyup(function(e) {
      if (e.keyCode===27) {
        LxDialogService.close(elem);
      }
    });
  };
```

The sidebar is created using `flexbox` and is shown only if there is enough
space on the screen.

```html
      <!-- sidebar -->
      <div flex-item="2" flex-item-order="1">
        <div class="sidebar sidebar--shown" ng-class="{'sidebar--shown': isVisible()}">
          <div class="sidebar-menu">
            <ul>
              <li><a class="sidebar-menu__link">Sidebar</a></li>
            </ul>
          </div>
        </div>
      </div>
      <!-- end sidebar -->
```

Further description of the UI is out of scope of this tutorial, but each element
is well documented elsewhere.

To wrap up this app, a
[manifest.json](https://developer.chrome.com/extensions/manifest) file is added,
to provide machine-readable information about this page. For example, one
particularly useful function in the Chrome browser is to use the menu option
"Create application shortcuts" which will package a webpage as an app that can
be run on desktop or mobile.

The [DOAP (Description of a Project)](https://github.com/edumbill/doap/wiki)
system is used to add linked data fields to an app. It is stored in `doap.ttl`
which Github Pages conveniently serves using the mime type `text/turtle`.
Auto-discovery is performed using the line

```html
  <meta href="doap.ttl#this" rel="http://www.w3.org/ns/solid/app#configuration">
```

In the file you will find descriptions of the solid app:

```html
<#this>
    a <http://usefulinc.com/ns/doap#Project>, <http://www.w3.org/ns/solid/app#Configuration> ;
    <http://usefulinc.com/ns/doap#description> "A Video Saving for the Solid platform" ;
    <http://usefulinc.com/ns/doap#download-page> <https://melvincarvalho.github.io/video/> ;
    <http://usefulinc.com/ns/doap#homepage> <https://melvincarvalho.github.io/video/> ;
    <http://usefulinc.com/ns/doap#license> "mit" ;
    <http://usefulinc.com/ns/doap#maintainer> <http://melvincarvalho.com/#me> ;
    <http://usefulinc.com/ns/doap#name> "Video" ;
    <http://usefulinc.com/ns/doap#shortdesc> "A Video App for the Solid platform" ;
    <http://usefulinc.com/ns/doap#shortname> "Video" ;
    <http://www.w3.org/ns/solid/app#description> "A Video App for the Solid platform" ;
    <http://www.w3.org/ns/solid/app#homepage> <https://melvincarvalho.github.io/video/> ;
    <http://www.w3.org/ns/solid/app#icon> <http://melvincarvalho.github.io/video/images/icon.png> ;
    <http://www.w3.org/ns/solid/app#name> "Video" .
```

This enables Linked Data app stores to process your app, copy and install it to
new spaces.

When all this is combined you will see a running something like:

[Solid Video Live Demo](http://melvincarvalho.github.io/video/)

# Summary

In this tutorial we showed how to embed video in a page. Some more advanced UI
techniques such as sidebar, menu and dialog boxes were covered. We also showed
how to make your app semantically rich using manifests, DOAP and auto discovery.
In the next tutorial we will expand on these patterns and create a more complex
game type app.

## See Also

* [Source Code](https://github.com/melvincarvalho/video)
* [Live Demo](http://melvincarvalho.github.io/video/)
* [SIOC](http://rdfs.org/sioc/spec/)
* [manifest.json](https://developer.chrome.com/extensions/manifest)
* [DOAP](https://github.com/edumbill/doap/wiki)
* [CORS](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing)

#### Chapter 4 - Solid Words

![](https://melvincarvalho.gitbooks.io/solid-tutorials/content/words.png)

## Introduction

In this tutorial we will expand on the previous Video tutorial, and show you
how to create a useful app for word training and vocabulary.

*What you will learn*

* How to structure JavaScript into several parts
* How to set up a Turtle data file on github
* How to use `localStorage` to save state
* How to automatically remember a user and login
* How to prepare language files to be used in vocab training
* How to add audio to apps

## The App

Next, we will create an app that is used to learn vocabulary in a foreign
language. A file is prepared containing the 10,000 most common words in a target
language. The source for word frequency was
[Wiktionary Frequency Lists](https://en.wiktionary.org/wiki/Wiktionary:Frequency_lists).
For this demo I have used the Czech and English languages, but any language pair
is possible. The app will test you randomly on the 1000 most common words. You
can click on the word if you don't know it, and a translation will momentarily
appear. If you know the word you can click 'easy' if you don't know the word,
click 'again', and if you almost know it, click 'good'. This is a typical style
for [Spaced Repetition Memorization](https://en.wikipedia.org/wiki/Spaced_repetition)
style learning.

The number of words can be cycled to 2000, 3000, 5000 or 10000 as you progress
using the changer on the left. Or an arbitrary number can be tested using the
max query string parameter. Displayed next to the graph is how difficult the
word is in terms of frequency. As you make more trials the number of words
increases and your percentage right. It is possible to reset the number by
tapping it.

From a list of words it's possible to paste it into an online translator and get
a list of translations (provided by Google Translate). A simple program can
change a 2 column text file into the needed Turtle format.
I've used `awk` for this:

```
awk -F $'\t' ' { print "<#" ++i "> <http://www.w3.org/2000/01/rdf-schema#label>" " \""  $2 "\"" "@en .\n" "<#" i ">  <http://www.w3.org/2000/01/rdf-schema#label>" " \""  $1 "\"" "@cs ." }'
```

Which should give you output similar to this:

```html
    <#1512> <http://www.w3.org/2000/01/rdf-schema#label> "žádost"@cs .
    <#1512> <http://www.w3.org/2000/01/rdf-schema#label> "application"@en .
````

One line is in Czech (cs) one in English (en). This file can be conveniently
stored as a .ttl in `gh-pages` on Github, so that it can be
[loaded](https://github.com/melvincarvalho/data/blob/master/vocab/czech.ttl)
into the app dynamically. This is a great way to host all sorts of Linked Data
content.

A number of things are persisted in `localStorage`: the word totals in each
bucket (easy, good and again), your user id, and the word lists. This is used to
log you in automatically on future usage of the app

```javascript
    if (localStorage.getItem('user')) {
      var user = JSON.parse(localStorage.getItem('user'));
      $scope.loginSuccess(user);
    }
```

The app is divided into sections for init, auth, fetching, rendering and helper
functions. This will be a typical division of functions for more complex apps.

Audio has also bee added to the buttons using the
[ngAudio](https://danielstern.github.io/ngAudio/#/) function and a simple tag
in the html

````html
   "ng-audio="audio/button-3.mp3"
```

Putting these pieces together creates a complete and quite useful Solid
application:

[Solid Words Live Demo](http://melvincarvalho.github.io/vocab/)

While the English-Czech language pair was used as default, any word file could
be used for vocabulary testing.

## Summary

In this chapter, we learned how to put together a complex set of flows into an
app structure that is more robust. `localStorage` was used to persist common
items, and login was remembered. Turtle files were created, hosted and pulled in
dynamically, and audio was added. In the next tutorial we will show how to
create realtime updates using
[Websockets](https://en.wikipedia.org/wiki/WebSocket).

## See Also

* [Source Code](https://github.com/melvincarvalho/vocab/)
* [Live Demo](http://melvincarvalho.github.io/vocab/)
* [wictionary](https://en.wiktionary.org/wiki/Wiktionary:Frequency_lists)
* [Spaced Repitition](https://en.wikipedia.org/wiki/Spaced_repetition)
* [ngAudio](https://danielstern.github.io/ngAudio/#/)

#### # Chapter 5 - Solid Chess

![](https://melvincarvalho.gitbooks.io/solid-tutorials/content/solidchess.png)

## Introduction

In this tutorial we will cover how to play a game of chess in realtime using
Solid and [Websockets](https://en.wikipedia.org/wiki/WebSocket) updates. A
JavaScript board is embedded in the page, updates occur in realtime, and a link
is provided to a chess engine to enable hints.

*What you will learn*

* How to add [Websockets](https://en.wikipedia.org/wiki/WebSocket) support
* How to send moves to a Pod in realtime
* How to update the board when a new move is made remotely
* How to create your own vocabulary
* How to embed a JavaScript chess widget in a page
* How to link to a chess engine for hints

## The App

The main aspect of this tutorial is realtime updates via Websockets. Solid uses
a pub/sub mechanism to allow users to subscribe to a resource, and will send out
updates when one of those resources changes.

[Websockets](https://en.wikipedia.org/wiki/WebSocket) are built into the browser,
and are started using the:

```javascript
    new WebSocket(uri)
```

syntax. After opening the socket, we then have access to the `onopen`,
`onclose`, `onerror` and `onmessage` functions. When a socket closes or errors,
we would like to restart it by calling a restart function.

```javascript
    socket.onerror = function(){
      console.log('socket error');
      setTimeout(connect, RECONNECT);
    };
```

After a socket has been opened, we will send a subscription to a resource:

```javascript
    socket.onopen = function(){
      console.log(sub);
      $scope.socket = socket;
      socket.send('sub ' + sub, socket);
      if (!quiet) {
        setInterval(function() { socket.send('ping'); }, INTERVAL);
      }
    };
```

Additionally, some web servers silently time out if not periodically pinged, so
we will send a ping message every 4 minutes. If we get a message, we check for
the 'pub' command and if found we will fire a callback for further processing.

```javascript
    socket.onmessage = function(msg) {
      var a = msg.data.split(' ');
      if (a[0] !== 'pub') return;
      processSocket(a[1]);
    };
```

When we get a message, the server will tell us which resource updated. So we can
now drop the cache, notify the user and fetch the resource again.

```javascript
  function processSocket(uri) {
    $scope.invalidate(uri);
    $scope.fetchBoard();
    $scope.audio.play();
  }
```

More Coming soon ...

[Solid Chess Live Demo](http://melvincarvalho.github.io/chess/)

## See Also

* [Source Code](https://github.com/melvincarvalho/chess)
* [Live Demo](http://melvincarvalho.github.io/chess/)
* [Websockets](https://en.wikipedia.org/wiki/WebSocket)

#### Chapter 6 - Solid Explorer

![](https://melvincarvalho.gitbooks.io/solid-tutorials/content/solidexplorer.png)

## Introduction

Explorer is a tool for exploring an debugging Solid workspaces.

*What you will learn*

## The App

More Coming soon ...

## See Also

* [Source Code](https://github.com/melvincarvalho/explorer)
* [Live Demo](http://melvincarvalho.github.io/explorer/)

#### Appendix B - SoLiD Discovery

# SoLiD Discovery (Draft)

The following describes discovery in the SoLiD framework using HTTP link following aka "follow your nose".

### Starting point -- WebID

The starting point of SoLiD discovery is a [WebID](http://www.w3.org/2005/Incubator/webid/spec/identity/) user profile, which is a hash based URI, typically denoting a (FOAF) Agent.  From this profile all of your storage can be found (discovery).

### Storage Discovery

#### Storage

* Starting Point: WebID
* Type: [pim : storage](http://www.w3.org/ns/pim/space#storage)

Storage is a root place to store files and data, which is used to create workspaces (see below).  Storage can also be of type:
* Public (everyone can see)
* Personal (only the user can see)
* Controlled (allows sharing)

#### Workspaces

* Starting Point: WebID or preferencesFile
* Type: [pim : workspace](http://www.w3.org/ns/pim/space#workspace)

Workspaces are where data is stored.  There are various types of workspaces, which normally will have their own type.  For example workspaces can be
* Personal (only the user can see)
* Private (only the user can see)
* Shared (allows sharing)
* Master (a workspace about other workspaces)

#### App Configuration Workspace

* Starting Point: WebID or preferencesFile
* Type: [pim : workspace](http://www.w3.org/ns/pim/space#workspace)

The app configuration workspace is a container of many different app configurations.  It is also possile to use the "glob * " function, for convenience, to get all configurations of various apps that are in use.

#### Preferences File

* Starting Point: WebID
* Type: [pim : preferencesFile](http://www.w3.org/ns/pim/space#preferencesFile)
* Access: Private

Preferences File is a private file that is linked from your WebID, and contains miscellaneous data not in your public profile.


#### App configuration Files

* Starting Point: App Configuration Workspace
* Type: [pim : ConfigurationFile](http://www.w3.org/ns/pim/space#configurationFile)

App configuration files contain all information related to an app.  


### Ontologies

* pim : http://www.w3.org/ns/pim/space#


### Illustration

![discovery illustration](assets/discovery.png "discovery illustration")\

* [Yo Tutorial](https://github.com/RubenVerborgh/solid-yo/blob/612043e9a34c6ad4dc51651b14136175776aa52e/scripts/YoSource.js#L69-L87)
