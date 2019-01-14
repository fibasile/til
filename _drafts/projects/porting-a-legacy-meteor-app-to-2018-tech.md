---
layout: article
title: Porting a legacy Meteor app to 2018 tech
tags:
- meteor
- react
- strapi
- oauth
date: ''
category:
- frontend
- backend
- projects

---
Few years ago I created a job board for the Fab Lab network, [http://jobs.fabeconomy.com.](http://jobs.fabeconomy.com. "http://jobs.fabeconomy.com.")

This helped quite a few people finding a job in the network, and I’m quite happy about that.

During the years we found out there are some annoying issues in the current code.

Which I never spent much time to fix, partially due to the technology choices I made when I built it.

Actually I did just one update, when a new version of meteor accounts-ui broke my app two years ago, but nothing else since then.

As you can figure out from the title, the app is written in Meteor, so both client and server share the same code.

While I appreciated (and still do) the easy path created by the Meteor team when starting, I now regret being tied to something that only runs on Node 0.10.x and doesn’t leave me other option than rewrite everything from scratch, both on the server and the client side.

Meteor now has evolved into something more robust and stable. Many people use it in production and are happy about it. From my side, I'll try to upgrade this app to a more modern stack, with a clear separation between front-end and backend. Feels like going back in time, but I still see this is a much more effective approach, especially if I need to add a mobile app in a later time.

## Moving to a modern stack

I want this app to grow and be a pleasure to extend and maintain, also I would love to have people using it for other projects, creating job boards with a single line of docker-compose, now.sh, or Heroku would be also great.

One the good side, I still can reuse much of the database structure defined few years ago. Still I now have a choice on pretty much everything else in the technology stack. Let’s figure out the options.

### Database: MongoDB vs Firebase vs SQL

#### Mongodb

PRO

* If I stick to Mongodb I won’t need to import data, and can prototype on a copy of the real data.
* I can have control on pretty everything
* I can do search on the db (*)
* I can use Mongoose to simplify model access, most of the existing Meteor db-code should be easy to reuse
* Everybody uses it, lots of tutorials for any framework
* Scalability

CONS

* I need to manage and backup a db in the cloud, unless I use some existing MongoHQ account payed by other projects
* No realtime db

#### Firebase

PRO

* Assuming I use the new Firestore (some people report issues on the old firebase storage) I can totally forget availability and backups
* Nice tools, support for easy Authentication
* Realtime db
* Everybody uses it, lots of tutorials for any framework
* Unlimited scalability

CONS

* The hierarchical structure of Firebase is a bit different from traditional Nosql in my opinion, models should be adapted a little
* This makes also a bit more work to import existing data
* No search, unless you brew your own or integrate Elastic search
* Cost?

### SQL Dbs - Postgres

PRO

* rock solid
* tons of extensions
* good library support

CON

* rigid structure for the DB unless you want to deal with migrations
* overkill for a few tables
* scalability?

## Backend framework

Lots of options. I’m inclined not use any “complex” framework or something I can rewrite it from scratch.

So I’ll limit the options to anything that can output a REST API and handle token-based authentication.

* Express
* Loopback
* Koa
* Micro
* Hapi
* Strapi
* Other non Javascript frameworks: Flask, Rails - they are still cool in 2018 (!)

Most of the frameworks in the list are pretty popular. I tried most of them in many little APIs and projects, so here's a summary:

* Express is like tango. You need to learn it because it might come useful some day. Actually pretty often, since this is the standard way of doing backends apps in Node, and all the frontend tools support Express as a wrapper.[ I suggest to read this article about node_modules madness.](https://medium.com/s/silicon-satire/i-peeked-into-my-node-modules-directory-and-you-wont-believe-what-happened-next-b89f63d21558 "What Happened When I Peeked Into My Node_Modules Directory")
* Loopback is an enterprise-level framework, really great for supporting complex schemas and data coming from different sources. I have a backend in production from few years, and it didn't give much trouble at all. The policies are very granular and you can benefit from a dynamically generated API and documentation.
* Koa is an evolution of Express. I see this as a good alternative if you want to write your api in ES6 with asyncs and awaits.
* Hapi is specialized in APIs. I never used it, mostly because its feature set is similar to express or Koa.
* Strapi is the new kid on the block. It provides REST api,  Admin framework, role-based access control and other popular features. But the thing that impressed me more is the fact that all the API code is scaffolded rather than inherited from the library. This way you can adapt the code to your needs, trust it more, and eventually grow it according to the app needs. For example you can quickly code in your own persistence layer, or even integrate an external data provider, instead of the existing database only in the places where the app needs it. Authentication with most OAuth providers is really a plus.

In short, I'll again favour the new kid on the block over the more stable solutions and see what happens. Will Strapi work in 5 years? 

Well this isn't really a bet. If Strapi will be abandoned or dropped in favour of something fancier, most of my code this time ( business logic, data persistance ) will still run under express or similar, and the front-end code will be independent and so reusable. Only the roles and auth would need to be redone.

## Frontend framework

Here the choice is huge. I can pick either Vue.js or React and be “modern”. I did something with both, so my choice will be influenced by the UI framework mostly, as I'm aiming for a Material look-and-feel.

* Vue.js - Simple, effective, and in my opinion much more fun to work with than React. It as a large ecosystem, but limited compared to React. On the UI Framework side, I enjoyed both Element-UI (used for my [Evaluation](http://nueval.fabacademy.org) app) and Quasar framework, which I used for the [Fab 14 Conference mobile app](https://app.fabevent.org). While the former is very complete, still I found many instabilities in the form elements, and the latter is basically a spaceship, allowing to generate any kind of desktop, web and mobile interface. I really like Vue and these projects. I see Vue.js as React "distilled".
* React - Everybody does some kind of React today, either on the web or on mobile. The ecosystem is so huge, something that can be compared only to jQuery or PHP one. React is ugly compared to Vue.js. Ugly in terms of complexity, in terms of too many options to do one single thing. But very powerful, and development is made a little easier by the ton of tutorials and great development - debugging tools available. On the UI Side, there's also a lot to try out. The most interesting project for me is Material-UI, because of the documentation and the quality of the components provided by this library.

The last two major web apps I built from scratch were both Vue projects. So I'll give a chance to React and

In this case Material-ui and React look like a good combination.