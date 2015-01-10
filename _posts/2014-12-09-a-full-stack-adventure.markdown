---
layout: post
title: "Trying out MEAN.JS"
excerpt: "Exploring the MEAN stack and general web development"
date: 2014-12-09T03:00:11-05:00
modified: 2015-01-07
---

## Section 1: Practical Motivation

It has been a while since my last post. For the past few months, I have been busy helping out the family business. 
On a high level, I have been serving as a consultant for the business handling everything from finances, marketing, communication,
technical support and web development.

When my uncle partnered with a local laundry delivery business, this business had a extremely out of date website.
Compelled by curiosity and a do-it-yourself ethic, I decided to create a new and comparatively modern website.
I wrote out this guide to document the experience and to perhaps help other curious minds navigate modern web development.

You can view the finished product [here](http://http://www.usmommy.com/#!/). There is nothing particularly innovative about this web application; it is a simple and modern version of the client's outdated static web page with some dynamic components. I tried to update the text content to be more succinct and concise, however the client insisted on having the content you see now.

## Section 2: Technology Stack

+ [VirtualBox](https://www.virtualbox.org/) to host a guest Linux OS on my Windows 8 laptop.
  *  I highly recommend installing [Guest Additions](https://www.virtualbox.org/manual/ch04.html) and working in seamless mode.
  *  Guest Additions allows for conveniences such as mouse pointer integration, shared folders and shared clipboard between host and guest.  
  <br>

+ [Linux Mint](http://www.linuxmint.com/) is the Linux OS I chose, though any would be fine.
  * There are numerous desktop environments like Cinnamon, MATE, KDE and XFCE. 
  * I chose XFCE, the arguably lightweight environment to minimize the resource consumption by VirtualBox.
  * Virtual Box Guest Additions must also be installed on the guest operating system.  
  <br>

+ [MEAN.JS](http://meanjs.org/), an open-source Javascript based full-stack framework.
  * MongoDB: Cross-platform document-oriented NoSQL (not only SQL) database.
  * ExpressJS: Node.js web application framework.
  * AngularJS: Front-end MVC framework.
  * NodeJS: Cross-platform runtime environment for server-side and networking applications.
  * Check the MEAN.js page for resources on each layer of the stack.  
  <br>

+ [DigitalOcean](https://www.digitalocean.com/), simple cloud hosting service for the application.
  * I found "Free Two Months Hosting" coupons on RetailMeNot.  
  <br>

+ [GitHub](https://github.com/) for source control and deployment to the Digital Ocean Droplet™.
  * Read [this](http://nvie.com/posts/a-successful-git-branching-model/) awesome post for a good Git branching practices.
  * I develop locally, push to repository and pull on remote.  
  <br>

+ [NameCheap](https://www.namecheap.com/) is where I purchased the domains.
  * I used two promo codes to lower the yearly price from ~$11 to ~$9.  
  <br>

+ [nginx](http://nginx.org/), pronounced "engine x", is an HTTP and reverse proxy server.
  * Node default listens to port 3000, nginx proxies requests from port 80 to the underlying port 3000.
  <br>

## Section 3: Getting Started

Each of these tasks can take a few hours to complete, especially if you run into any installation or setup issues. Please take your time and make sure each step is completed before moving on. Google and StackOverflow are great resources for troubleshooting and debugging! The following steps assume the host machine runs Windows. If you are already using a Linux-ish operating system, then please skip to step 4.

1. Install VirtualBox, please refer to the [user manual](http://www.virtualbox.org/manual/) for assistance.
2. Download the .iso of your preferred flavor of Linux. View popular options at [DistroWatch](http://distrowatch.com/dwres.php?resource=major).
3. Install VirtualBox Guest Additions on both the host OS and the guest OS.
4. Install MEAN.JS and its [prerequisites](http://meanjs.org/docs.html#getting-started): Node.js, npm, MongoDB, bower, grunt.
5. [Generate](http://meanjs.org/generator.html) your project directory, which initializes with a sample MEAN stack application.

## Section 4: Preemptive Development Advice

### Grep (globally search a regular expression and print)

{% highlight bash %}
# front-end content you want to edit, but unsure of file location: grep it
# find any file dependencies in unfamiliar directories: grep it

grep -r "texthere" .
# execute in root project directory
{% endhighlight %}

### Spelling

If your application passes the linter, but is not functioning, check your variable spelling.

### Incremental Development

Work on this project incrementally. Move on to the next section when you understood the current section. This sounds obvious, however it is definitely worth stating and emphasizing. A web application is not as complicated or daunting when you understand its components.

### Version Control

If you decide to create a web application or any non-trivial application without any version control, then you are a very very brave individual. 

## Section 5: The Full-Stack Nitty Gritty

I studied computer science, however that does not mean I formally learned web development. Over the the course of my undergraduate years I did study the fundamental components that comprise basic web development: programming languages, computer architecture, databases, operating systems, networks and security. With that stated, a working knowledge of the Linux environment, HTML and Javascript is necessary for the upcoming sub-sections.

**Halt!** Take your time working through this. Take breaks to digest and understand what is going on. 
{: .notice}


### 5A: Setting Up Local Development Environment

Before doing anything, you must start the **mongod** service if it is not already started.
You can check by running any of the following in your Terminal:

{% highlight bash %}
# pROCESS sTATUS
$ ps aux | grep mongod
	--or--
$ /etc/init.d/mongod status
	--or--
$ service mongod status
{% endhighlight %}

For future reference, if you are unfamiliar with any of these Terminal commands, then check the **man** pages.

{% highlight bash %}
$ man ps
{% endhighlight %}

Now, start **mongod**, "the primary daemon process for the MongoDB system" !

{% highlight bash %}
$ mongod
[initandlisten] MongoDB starting : pid=6207 port=27017 dbpath=/data/db 64-bit host=zenbook
[initandlisten] db version v2.6.6
...
[initandlisten] ERROR: Insufficient free space for journal files
[initandlisten] Please make at least 3379MB available in /data/db/journal or use --smallfiles
[initandlisten] 
[initandlisten] exception in initAndListen: 15926 Insufficient free space for journals, terminating
...
[initandlisten] dbexit: really exiting now

{% endhighlight %}

If you do not have sufficient free space like in the above example, then do:

{% highlight bash %}
$ mongod --smallfiles
[initandlisten] MongoDB starting : pid=6084 port=27017 dbpath=/data/db 64-bit host=zenbook
[initandlisten] db version v2.6.6
...
[initandlisten] waiting for connections on port 27017
[clientcursormon] mem (MB) res:83 virt:728
[clientcursormon]  mapped (incl journal view):544
[clientcursormon]  connections:0
{% endhighlight %}

Open a *new* Terminal window or tab and execute:

{% highlight bash %}
# grunt is a JS task runner used to automate tasks like linting and minification
# grunt conveniently relaunches the application when it detects any saved file changes
# view gruntfile.js in the root project directory for more information
$ grunt
Running "jshint:all" (jshint) task
>> 56 files lint free.

Running "csslint:all" (csslint) task
>> 2 files lint free.

Running "concurrent:default" (concurrent) task
Running "watch" task
Waiting...
Running "nodemon:dev" (nodemon) task
[nodemon] v1.2.1
[nodemon] to restart at any time, enter `rs`
[nodemon] watching: app/views/**/*.* gruntfile.js server.js config/**/*.js app/**/*.js
[nodemon] starting `node --debug server.js`
debugger listening on port 5858

 NODE_ENV is not defined! Using default development environment

MEAN.JS application started on port 3000
{% endhighlight %}

You may specify the local development NODE_ENV, however it does use the development environment by default.

{% highlight bash %}
# environments are in /.../config/env
$ export NODE_ENV = development
{% endhighlight %}

Open up a web browser in your Linux environment and go to localhost, port 3000:

{% highlight html %}
http://localhost:3000/
{% endhighlight %}

Spend some time familiarizing yourself with the sample application and please take note of the different *modules* in the application. At the very least create an account, log in, create articles on your account and log out.


### 5B: Front-end, the Web UI Layer

From exploring the sample application, you should have noticed the *core* components of the website like the header, the *user* components like the log-in forms, and the *articles* components for interacting with user articles. The **user interface** of these components are organized by isolated modules. All front-end related files are in **/.../public** and as you can see below:

{% highlight html %}
/public
   /dist
   /lib
   /modules
      /articles
         ...
      /core
         /config	-- AngularJS routing
         /controllers	-- AngularJS controllers
         /css
         /img
         /services	-- AngularJS services
         /tests			
         /views		-- html files
         ...
      /users
         ...
      ...
   ...
{% endhighlight %}

To test this setup, go into **/.../views** and edit the homepage with your preferred text editor or IDE.

{% highlight bash %}
$ cd /path/to/your/project/project_name/public/modules/core/views/home.client.view.html
{% endhighlight %}

After you update the static content, save the file and *grunt* will automatically update the application. Please use [Bootstrap](http://getbootstrap.com/) for all of your basic user interface needs.

To achieve more dynamic front-end content, you have AngularJS. If you are unfamiliar with Angular and MVC patterns, then now is a great time to follow [this tutorial](https://docs.angularjs.org/tutorial) from the official AngularJS website. I spent a few hours a day spread across an entire week going through the tutorial and experimenting with different examples before I felt comfortable. Extrapolate the newly learned concepts and apply them to the MEAN.JS structuring.

**Halt!** Take a break to rest your body and mind. Get up from your chair and stretch.
{: .notice}

### 5C: Back-end, the Node.js Server

Node.js is the server for this web application. In order to create an account, log-in and log-out, the user interacts with the server from their client. Creating, reading, updating and deleting articles also requires interacting with the server. Web application users should be able to log-in into their accounts and interact with their articles from any computer; no data is stored locally. All server code is located in **/.../app**.
	
The MEAN.JS stack uses Express, which is a "minimal and flexible" Node.js web application framework. It, like most Node.js libraries, can be installed with npm. Express simplifies the process of writing server code by abstracting away the tedious and complex parts. For more information, read the super informative post by [Evan Hahn](http://evanhahn.com/understanding-express/). It really helps to understand what Express does as many of the sample and toy Node.js applications I found online heavily used it.

{% highlight html %}
/app
   /controller     -- contains Express application controllers; back-end business logic
   /models         -- contains Mongoose models; this is where you define your back-end models
   /routes         -- contains the Express server routing configuration files
   /tests
   /views
   ...
{% endhighlight %}

You should directly see the articles controller code in the **articles.server.controller.js** file. Any code involving 'mongoose' or 'model' are database related. I am not going to delve into the database aspects beyond stating that Mongoose is a Node.js library for interacting with MongoDB. Anyhow, please spend a few moments to read and understand the the code. I have shared a snippet of the file below.

{% highlight js %}
'use strict';

/**
 * Module dependencies.
 */
var mongoose = require('mongoose'),
	errorHandler = require('./errors'),
	Article = mongoose.model('Article'),
	_ = require('lodash');

/**
 * Create a article
 */
exports.create = function(req, res) {
	var article = new Article(req.body);
	article.user = req.user;

	article.save(function(err) {
		if (err) {
			return res.status(400).send({
				message: errorHandler.getErrorMessage(err)
			});
		} else {
			res.jsonp(article);
		}
	});
};

/**
 * Show the current article
 */
exports.read = function(req, res) {
	res.jsonp(req.article);
};

/**
 * Update a article
 */
exports.update = function(req, res) {
	var article = req.article;

	article = _.extend(article, req.body);

	article.save(function(err) {
		if (err) {
			return res.status(400).send({
				message: errorHandler.getErrorMessage(err)
			});
		} else {
			res.jsonp(article);
		}
	});
};

/**
 * Delete an article
 */
exports.delete = function(req, res) {
	var article = req.article;

	article.remove(function(err) {
		if (err) {
			return res.status(400).send({
				message: errorHandler.getErrorMessage(err)
			});
		} else {
			res.jsonp(article);
		}
	});
};

.......

{% endhighlight %}

I used the articles and user controller code as a basis to write my own logic. Writing my own "business logic" is when I had the most fun in this project. The majority of my enjoyable programming experience has been in writing "back-end" code. I am most comfortable with Java and have working knowledge of C and Python. Finally being able to write basic functions was a great motivation boost.

**Halt!:** Feeling bleary-eyed? Take a break, have a cup of tea and resume this later.
{: .notice}


### 5D: Integration

Integrating the front-end and back-end together produces a dynamic web application. Typically, the client interacts with a server by sending a http request and the server sends a http response. In a MEAN stack, Angular sends http **req**uests to the server and waits for a **res**ponse. Express **routes** the request to the proper **controller** to handle the request.

<figure>
	<a href="http://i.imgur.com/y3OvtwJ.png"><img src="http://i.imgur.com/y3OvtwJ.png"></a>
	<figcaption>Diagram illustrating the control flow of a MEAN stack web application.</figcaption>
</figure>

The following section has a concrete example of integrating the front-end and back-end.



## Section 6: Sample Contact Page with nodemailer 0.7.1

a. Create a contact page: **contact.client.view.html**

{% highlight html %}
<section data-ng-controller="ContactController">
<!-- Boostrap panels -->
<div class="panel panel-primary">
  <div class="panel-heading">
    <h3 class="panel-title">Email</h3>
  </div>
  <div class="panel-body">
	<div ng-show="!sent">
	  <form name="contactForm" ng-submit="processForm()" class="simple-form">
	    <!-- Our data model is called "guest" -->
		<div class="form-group">
		  <label>Name</label>  
		  <input type="text" data-ng-model="guest.name" class="form-control" 
		  placeholder="Your name here" required>
		</div>
		<hr>
		<div class="form-group">
		  <label>Email Address</label>
		  <input type="email" data-ng-model="guest.email" class="form-control" 
		  placeholder="Your email address here" required/>
		</div>
		<hr>
		<div class="form-group">
		  <label>Phone Number</label>
		  <input type="phone" data-ng-model="guest.phone" class="form-control" 
		  placeholder="Your phone number here" required/>
		</div>
		<hr>
		<div class="form-group">
		  <label>Inquiry</label>
		  <textarea type="text" data-ng-model="guest.inquiry" class="form-control" 
		  rows="5" placeholder="Your message here" required/> </textarea>
		</div>
		<div class="pull-right">
		  <button class="btn btn-primary btn-lg">Submit Inquiry</button>
		</div>
	  </form>
	</div>
	<div ng-show="sent">
	  <div class="alert alert-success" role="alert">
		Thank you, your inquiry has been submitted.
	  </div>
	  <button class="btn btn-success btn-lg" ng-click="sent = !sent">
	  Submit Another Inquiry!
	  </button>
	</div>
</div>
</section>
{% endhighlight %}


b. Create ContactController: **contact.client.controller.js**

{% highlight js %}
'use strict';
angular.module('core').controller('ContactController', ['$scope', '$http', '$location',
  function($scope, $http, $location) {

  	// the naming of our data model is consistent
	$scope.guest = {};
	$scope.sent = false;

	// when the user submits it triggers processForm()
	$scope.processForm = function() {

	  // *** IMPORTANT IMPORTANT ***
	  // an http request is posted to the server
	  // *** IMPORTANT IMPORTANT ***

	  $http.post('/sendContact', $scope.guest)
	  .success(function(response) {
		  console.log('Success! :D');		

		  //logs to browser console (Chrome F12)

	  }).error(function(response) {
		  console.log(response);
	  });

	  $scope.contactForm.$setPristine();
	  $scope.guest = {};
	  $scope.sent = true;
	};
  }]);
{% endhighlight %}


c.  Route Angular controller to view: update **core.client.routes.js**

{% highlight js %}
'use strict';

// Setting up route
angular.module('core').config(['$stateProvider', '$urlRouterProvider',
  function($stateProvider, $urlRouterProvider) {
	// Redirect to home view when route not found
	$urlRouterProvider.otherwise('/');

	$stateProvider
      .state('home', {
        url: '/',
        templateUrl: 'modules/core/views/home.client.view.html'
      })
      .state('contact',{
        url: '/contact',
        templateUrl: 'modules/core/views/contact.client.view.html'
      });
    }
]);
{% endhighlight %}

d. Add contact page to header: update **header.client.view.html**

{% highlight html %}
<ul class="nav navbar-nav navbar-right">
    <li ui-route="/contact" ng-class="{active: $uiRoute}">
        <a href="/#!/contact">Contact</a>
    </li>
</ul>
{% endhighlight %}

e.  Setup Express to route /sendContact request to controller: **core.server.routes.js**

{% highlight js %}
'use strict';

var core = require('../../app/controllers/core');

module.exports = function(app) {
	
	// Root routing
	app.route('/').get(core.index);

	// Receives /sendContact request and calls the sendContact function
	app.route('/sendContact').post(core.sendContact);

};
{% endhighlight %}


f. Create a function to send a email: **core.server.controller.js**

{% highlight js %}
'use strict';

/**
 * Module dependencies.
 */
var mongoose = require('mongoose'),
errorHandler = require('./errors'),
Contact = mongoose.model('Contact'),
nodemailer = require('nodemailer'),
_ = require('lodash');

// you may have you downgrade your version of nodemailer
// MEAN.JS uses a more recent version that is incompatible

exports.index = function(req, res) {
  res.render('index', {
	user: req.user || null
  });
};

// update with your gmail information
// you may have to update your gmail settings
var gmTransport = nodemailer.createTransport('SMTP',{
  service: 'Gmail',
  auth: {
	user: 'your@gmail.address',
	pass: 'yourPassword'
  }
});

exports.sendContact = function(req, res) {

  // logs to grunt task
  console.log('sent inquiry');
	
  var mailOptions = {
	to : 'site@email.address',
	subject : 'Inquiry from: [' + req.body.name + ']',
	html: req.body.inquiry
  };

  gmTransport.sendMail(mailOptions, function(error, response){
	if(error){
  	  console.log(error);
	}else{
	  console.log('Message sent: ' + response.message);
	}
  });
};

{% endhighlight %}

g. Update (downgrade) the version of nodemailer in **package.json** to 0.7.1.

h. Test this locally by going to localhost and try using the contact form.

## Section 7: Deployment with Digital Ocean

Once you have completed testing locally, then it is time to deploy!

### 7A: Hosting Setup on DigitalOcean

a. Purchase domain (from namecheap).

b. Update "Domain Name Server Setup" and specify custom DNS servers.
![Namecheap Setup](http://i.imgur.com/SccNax1.png)

c. Sign up and log into DigitalOcean.

d. Create a droplet.

		1. Name your droplet.
		2. Select a size (some promotions offer $10 credits).
		3. Select your region.
		4. Select a linux distribution.
		5. Select the MEAN application.

e. Configure DNS on DigitalOcean
![DigitalOcean DNS](http://i.imgur.com/1CsqtJW.png)

f. Wait about 10-15 minutes for the DNS changes to propagate.

g. Ping the new domain.

{% highlight bash %}
$ ping yourdomain.com
PING yourdomain.com (12.34.56.789) 56(84) bytes of data.
64 bytes from 12.34.56.789: icmp_req=1 ttl=63 time=1.47 ms
64 bytes from 12.34.56.789: icmp_req=2 ttl=63 time=0.674 ms
{% endhighlight %}

### 7B: Launching the Droplet (server)

{% highlight bash %}
# 1. SSH into your droplet. Your password is emailed to you.
$ ssh root@111.111.111.111
-- or --
$ ssh root@yourdomain.com

# 2. Assuming you have been using source control, clone your application to the droplet.
$ cd /var/www
$ git clone https://github.com/yourGitHubName/yourProjectRepository

# 3. Set NODE_ENV
$ export NODE_ENV = "production"

# 4. Build your project:
$ grunt build

# 5. Install foreverjs 
$ [sudo] npm install forever -g

# 6. Execute forever to keep your application running (even after you exit the droplet).
$ forever start server.js
{% endhighlight %}

### 7C: Reverse Proxy with NGINX

The MEAN.JS application listens to port 3000 by default. When people access **yourdomain.com** they have to append the port making it **yourdomain.com:3000**, which is neither convenient or pleasant looking. By not specifying a port, **yourdomain.com** is actually the same as **yourdomain.com:80**. Whenever the user accesses the web application at **yourdomain.com**, their interactions should be passed from port 80 to port 3000. nginx, an HTTP and reverse proxy server, is exactly what we need to achieve that goal.

{% highlight bash %}
# install nginx
$ sudo apt-get update
$ sudo apt-get install nginx
{% endhighlight %}

The following is the setup I use.

{% highlight bash %}
# this redirects all non-www traffic to www
server {
    server_name  yourdomain.com;
    rewrite ^(.*) http://www.yourdomain.com$1 permanent;
}

# listens for requests to port 80 and passes it to port 3000.
server {

    listen 80;

    server_name www.yourdomain.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
{% endhighlight %}

If you like my nginx setup you can use it by doing the following:

{% highlight bash %}
$ cd /etc/nginx/sites-available

# replace the content of the "default" file with the above setup.
$ [sudo] vim default
-- or --
$ scp ...

# create a symlink of that file in site-enabled
$ ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled/default


{% endhighlight %}

After configuring nginx, restart it.

{% highlight bash %}
$ /etc/init.d/nginx restart
{% endhighlight %}

Now you should be able to visit **yourdomain.com** from anywhere!

## Section 8: Postmortem

This project was my first official "modern" web application. I previously made a Java servlet application, [YelpMeOut!](https://github.com/Kreispy/YelpMeOut), for a database project (CIS330 with Susan Davidson), but the web components of that project were cursory and not emphasized. This modern web application was initially overwhelming, confusing and difficult. At that point I just installed MEAN.JS and was not sure where or how to begin; I was aimlessly reading the sample project and trying to make sense of it. After getting my bearings, I spent time studying each individual layer of the MEAN stack, which was rather time consuming but overall straightforward. I thought getting each layer to interact with each other was going to be the greatest difficultly, however it was not. Using a full-stack framework allowed me to easily adopt good folder structures and proper routing.

[Recently](https://news.ycombinator.com/item?id=8669557) the core members behind the [Node.js](http://nodejs.org/) framework forked and created [io.js](https://github.com/iojs/io.js). These core members were unhappy with Joyent's overall management of the Node.js project. This is germane because MEAN.JS is a fork of the more popular MEAN.IO. You can read more about that [here](http://stackoverflow.com/questions/23199392/difference-between-mean-js-and-mean-io). In retrospect, I might have been able to complete this application sooner if I went with MEAN.IO. MEAN.IO, being relatively older and more well known, is more documented and supported (currently) than MEAN.JS. 

Completing this project has bolstered my confidence in my technical skills and has increased my general technical capabilities. Documenting the project from start to finish has made me more patient and also more appreciative of thorough documentation. Looking forward, there are a few sections of text that I would like to revise in the future (to make more stylistically consistent). I am still trying to determine my style of expository technical writing. It took more than three months worth of my free time to learn both the framework and general web development principles. It took about an additional month or so to digest and document my adventure. Since finishing this post, I started working on a new application in Java with the Play framework. Thank you for taking the time to read this post.