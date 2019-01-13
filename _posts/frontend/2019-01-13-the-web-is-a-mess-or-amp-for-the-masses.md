---
layout: article
title: The web is a mess, or AMP for the masses
tags: []
date: 2019-01-13 00:00:00 +0100
category: []

---
The web is a mess. 

If you look today at any modern website you will find combined at least a dozen different standards, all mixed up by Javascript adding complexity to complexity.

Thanks only to the recent [Chrome monopoly](https://www.w3counter.com/globalstats.php) account of more than 60% of users we are forgetting about the browser quirks and incompatibilities. 

Still every website is created with a different technology, uses different frameworks and a lot of websites are still made out of image maps (!) and in general not at all usable on many platforms.

Don't get me wrong, you can use most of the web with a (modern) desktop browser. But it is a totally different story when you use a mobile browser, or if you are masochist, a smart tv embedded browser.

Another pain point is that creating a web browser today is really hard. And the browser itself has so many things to do, using tons memory and power. The latter makes browsing one of the most power hungry applications on mobiles, which is unfortunately how most people spend their time online.

Maybe I'm getting old, but sometimes I miss the Netscape-era, or even [Gopher](https://en.wikipedia.org/wiki/Gopher_(protocol)) (yes, I'm that old) when you needed to read one or two specifications to be the master of the web. It felt powerful, but also pretty safe, as there's wasn't so much to mess up with in any case. Not very powerful computers did let you surf the web with pretty good speeds ( at least using the university of Pisa amazing -for 1993- 100Mb link).

I felt many times alone fearing I was the only one thinking that the only way to fix the web would be to redo it from scratch.

And what better opportunity for Google than being the leading search engine and browser provider to do it?

Announced in 2015 and launched in 2017, the [Accelerated Mobile Pages initiative (AMP)]() is a set of specifications, including AMP Html, AMP Email and AMP Stories. 

AMP introduced two main limits for web developers:

* Anything that would slow down page load times is forbidden, including external stylesheets and scripts.
* No javascript is allowed except AMP-specific modules that are provided along with a boilerplate page.
* You should include Schema.org definitions for better indexing of your content.

Amp extends / replaces parts of HTML with custom tags, including images, embeds, and also provides a responsive layout system.

According to the wikipedia page [AMP](https://en.wikipedia.org/wiki/Accelerated_Mobile_Pages) adoption is still quite controversial. Still all the bigs (except Facebook) have adopted it as standards for their mobile presence, such as Bing, Twitter, Baidu, eBay. 

 What AMP really gives us?

* a clean and standard framework for front end, especially when doing news / blogs and e-commerce
* a better placement in Google search results on mobile
* the guarantee that your site is as fast as possible, being served by the Google CDN, automatically, all over the world.

Why should I use AMP in 2019?

I like AMP simplicity, the total opposite of what current front-end technologies are.

I like that the technology is now stable and widely adopted.

I feel AMP is perfect for micro-blogging, news reporting, etc. But also for the cool thing of the moment - stories.

Introducing Jekyll AMP

I started a little pet project called [Jekyll AMP](https://fibasile.github.io/jekyll-amp) with the idea of creating a blank Jekyll theme ready to be used as a scaffold for AMP sites.

It's still in early development but I will post updates in the upcoming weeks.