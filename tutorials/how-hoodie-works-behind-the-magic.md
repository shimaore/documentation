# Behind the Magic – a detailed explanation how Hoodie works

This tutorial shows you how exactly all parts of Hoodie work together to create a seamless, consistent user experience.

## Table of Content
- <a href="#all-parts-of-hoodie">All parts of Hoodie</a>
- <a href="#how-it-works-example-sending-an-email">How it works: example – sending an email</a>

### All parts of Hoodie
When we build an app with Hoodie, we have three parts: frontend, backend, and they’re connected with each other through the Hoodie sync.
<p><img src="http://blog.hood.ie/wp-content/uploads/2014/07/Screen-Shot-2014-07-16-at-14.24.53.png" width="50%" height="50%"></p>

In the frontend, you have your app...
<p><img src="http://blog.hood.ie/wp-content/uploads/2014/07/Screen-Shot-2014-07-16-at-14.25.04.png" width="50%" height="50%"></p>

The app always only talks to the Hoodie API, never directly to the server-side code, database or even in-browser storage.
<p><img src="http://blog.hood.ie/wp-content/uploads/2014/07/Screen-Shot-2014-07-16-at-14.25.13.png" width="50%" height="50%"></p>

You can replace localstorage with any in-browser storage of your choice. Here all the data is saves locally. So we do not lose any data, if we are offline.
<p><img src="http://blog.hood.ie/wp-content/uploads/2014/07/Screen-Shot-2014-07-16-at-14.25.23.png" width="50%" height="50%"></p>

And at this point, you could already stop if you wanted. This, by itself, is already enough for an app. Still, if you want to save your data on a server, you'll need something in addition.

Hoodie relies on [CouchDB](http://couchdb.apache.org), the database that replicates. In CouchDB, each user has their own private database which only they can access. And all data is private by default. It can be shared to the public if the user decides to, but it can't happen by accident.
<p><img src="http://blog.hood.ie/wp-content/uploads/2014/07/Screen-Shot-2014-07-16-at-14.25.37.png" width="50%" height="50%"></p>

There are plugins based on node.js that bring Hoodie’s core features:
* user signup and administration
* data storage
* data loading & sync
* data shares // still in development
* emails
* payments. // still in development

These plugins are Hoodie's core plugins. In addition, anyone can build plugins themselves to extend Hoodie's core (see tutorial for [building plugins to extend Hoodie](https://github.com/hoodiehq/documentation/blob/gh-pages/tutorials/hoodie-plugin-tutorial/index.md)).
<p><img src="http://blog.hood.ie/wp-content/uploads/2014/07/Screen-Shot-2014-07-16-at-14.25.46.png" width="50%" height="50%"></p>

This means: with Hoodie, frontend and backend never talk directly to each other. They only leave each other messages and tasks. It’s all very loosely-coupled and event-based, which means it can be interrupted at any stage without breaking. It’s designed for eventual consistency.
<p><img src="http://blog.hood.ie/wp-content/uploads/2014/07/Screen-Shot-2014-07-16-at-14.25.59.png" width="50%" height="50%"></p>

--------

### How it works, example: sending a message

Let’s see how this works when an email is moving through the system:

1. The message is written in the app and send
2. The message is passed to hoodie.store and the task "save the message" is send to the localstorage to be saved. 
3. Also hoodie.store checks if the user is online. If so, the task is handed to the sync and through the REST layer the message is saved in our CouchDB database including the message. 
4. The CouchDB database gives a task to the Node.js plugin to send the message including the additional data and the message itself. 
5. The Plugin sends the message.

<p><img src="http://blog.hood.ie/wp-content/uploads/2014/07/Screen-Shot-2014-07-16-at-14.25.46.png" width="50%" height="50%"></p>