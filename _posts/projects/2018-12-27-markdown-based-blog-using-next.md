---
layout: article
title: Markdown based blog using next.js
date: 2018-12-27 20:29 +0100
category: projects
tags: 
    - markdown
    - nextjs
    - "static-generator"
---

Next.js is a new framework from Zeit allowing to quickly create SSR rendered React webapps.

I'm using the framework, along with Material UI, as the stack for the new http://oscircularfashion.com app. I like Material-UI for its extensive documentation and the polished feeling it gives to the webapp.  I might have used Vuejs, which I find a little tidier than React, if it wasn't for Material UI, but it's only available for React.

While I created most of the other app screens, I feel the need to get more indepth with Nextjs before going all in with the oscircularfashion. I hope also to learn some animation tricks to make it fancy.

I decided that a little markdown blog app will sharpen my Nextjs skills and be useful as a starting point for others.

## Setting up the skeleton

I'm using the Nextjs Material-ui example from:

https://github.com/mui-org/material-ui/tree/master/examples/nextjs

Note: following the instructions above will throw an error when starting ```npm run dev```.

**Solution**

The error is caused by react-jss not being installed. I changed the following line in `pages/_app.js`:

```javascript
import JssProvider from 'react-jss/lib/JssProvider';
```

with this:

```javascript
import { JssProvider } from 'react-jss';
```

And also installed `react-jss` in the top level forlder

```bash
$ npm install --save react-jss
```

## App reqirements

My idea is to replicate as much as possible Jekyll conventions, this way anybody could import old blogs without much hassle.

It also helps a lot having Jekyll as a starting point for defining requirements.

This means my microblog should be able at least to do the following:

- Reading a _config.yml
- Reading a _posts/ folder, parsing Markdown file names
- Extracting metadata and indexing posts from the frontmatter
- Supporting local links
- Supporting static assets
- Replicate using HOC components the main variables (like site.* and page.*) exposed by jekyll
- Generate category, archive, tags pages
- Statically exporting the full blog in html
- Support per-page layouts

Nice to have
- Data and Collections support
- Draft posts
- Timed posts
- Optimizing images
- SEO Tags
- Full text Search
- Date based archives (Month, Year)
- Support Liquid templates 
- Command line generator for new content