---
layout: article
title: Automate Jekyll page and post creation
categories: frontend
---

Jekyll is really convenient, if only it had a way to automatically create posts and pages from the command line, then it would be great!  This is a list of tools I've checked to automate things a little.


## Jekyll Composer

This is a [Jekyll plugin](https://github.com/jekyll/jekyll-compose) that allows to extend the Jekyll CLI with a couple of new commands.

```shell
  draft      # Creates a new draft post with the given NAME
  post       # Creates a new post with the given NAME
  publish    # Moves a draft into the _posts directory and sets the date
  unpublish  # Moves a post back into the _drafts directory
  page       # Creates a new page with the given NAME
```

I discovered I had to update Jekyll to the latest version in order to use it.

Running ```bundle update``` helps!

I'm not sure If I will use it in the end, but it's a nice tool.

## Visual Studio Code

Using Visual Studio code snippets should also be possible.

- [VS Code Jekyll Snippets Extension](https://marketplace.visualstudio.com/items?itemName=ginfuru.vscode-jekyll-snippets)

Snippets can be taken from:

- [Jekyll Cheat Sheet](https://learn.cloudcannon.com/jekyll-cheat-sheet/)