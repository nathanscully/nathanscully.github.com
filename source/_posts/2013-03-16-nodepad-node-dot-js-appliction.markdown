---
layout: post
title: "Nodepad - Node.js Appliction"
date: 2013-03-16 17:51
comments: true
categories: 
---
## Introduction:
With my new motivation(and commitment) to learn Javascript - specifically the [node.js](www.nodejs.org) platform I have been hunting for a few tutorials.

After completing some basic helloworld I found [daily.js](www.dailyjs.com) and their [nodepad](http://dailyjs.com/2010/11/01/node-tutorial/) application. 

It was written a few years ago and is now out of date with versions, and likely syntax so I am going to go through the tutorial and write my own version. All credits to dailyjs for the original material.

Nodepad will be a simple web based notepad application built using node.js and taking advantage of the express framework

##Requirements:


- Node.js - an obvious requirement!
- MongoDB - a NoSQL database thats getting a good wraps across the interwebs
- NPM - Node package manager - used to install and manage any additonal packages we require. 

##Setup:

Make a new working directory:
	mkdir nodepad
Install express
	npm install -g express
The -g flag installs this globally, allowing us to setup a skeleton application:
	express nodepad
Change into the nodepad directory and install the dependencies
	npm install
Update the package.json to include the required dependencies:
	{
	  "name": "nodepad",
	  "version": "0.0.1",
	  "private": true,
	  "scripts": {
	    "start": "node app"
	  },
	  "dependencies": {
	    "express": "3.1.0",
	    "jade": "*",
	    "less": "*",
	    "expresso": "*",
	    "mongoose": "*"
	  }
	}
Run the skeleton application and view it on http://localhost:3000/:
	node app.js
Close the process and then setup a mongodb database:
in a new terminal start the mongodb server:
	mongod
connect via:
	mongo
Create a new nodepad db:
	use nodepad

##Application:
Setup our first data model - we will use mongoose to wrap a class around our mongodb database. Save the following as models.js in the root of the nodepad folder.
	var mongoose = require('mongoose');

	mongoose.model('Document', {
	  properties: ['title', 'data', 'tags'],

	  indexes: [
	    'title'
	  ]
	});

	exports.Document = function(db) {
	  return db.model('Document');
	};
We can now use the model via the following in our app.js file:
	db = mongoose.connect('mongodb://localhost/nodepad')
	Document = require('./models.js').Document(db);
Be sure to add the mongoose dependency to the list of required modules:
	mongoose = require('mongoose');
Add the following  config to our app.js. This will enable us to use less to complie our css
	app.configure(function(){
	...
	  app.use(express.compiler({ src : __dirname + '/public', enable: ['less']}));
	});

This completes the basic setup of our application. The next post will develop the application futher. 