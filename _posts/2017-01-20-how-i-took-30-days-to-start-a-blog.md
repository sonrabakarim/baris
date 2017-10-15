---
title: How I took 30 days to start a blog
category: blog
last_modified_at: 2017-09-06
---

(And months later am still changing things)

> [...]blogging is a communication pattern that optimizes for the amount of awareness and influence that each keystroke can possibly yield. [...] If your choice is to invest keystrokes in an email to three people, or in a blog entry that could be read by those same three people plus more — maybe many more — why not choose the latter? Why not make each keystroke work as hard as it can?
> <cite><a href="https://blog.jonudell.net/2007/04/10/too-busy-to-blog-count-your-keystrokes/">Too busy to blog? Count your keystrokes.</a></cite>

This blog was started as an experiment to [maximise the reach of my keystrokes](http://www.hanselman.com/blog/DoTheyDeserveTheGiftOfYourKeystrokes.aspx) and improve my writing.

Here is my journal of the experience.

## Blogging platform: Jekyll and GitHub Pages

*Day 1:* The first step was choosing a blogging platform. I decided to go with [Jekyll](https://jekyllrb.com/) and [GitHub Pages](https://pages.github.com/).

I'm a developer, so [blogging using a plain text editor and Git](http://tom.preston-werner.com/2008/11/17/blogging-like-a-hacker.html) fits in easily with my everyday workflow.

Using a static HTML website built by Jekyll provides advantages in [simplicity, speed and security](https://www.smashingmagazine.com/2014/08/build-blog-jekyll-github-pages/#the-advantages-of-going-static). And with GitHub Pages, hosting my blog is free!

## Setting up Jekyll locally

*Day 4:* Many of the Jekyll blog setup guides recommend forking an existing Jekyll repository as a starting point. This is effectively a "deploy straight to production" approach which will get your site up and running quickly.

I wanted to be able to build and test the website before publishing it, so followed the steps for [setting up Jekyll and its dependencies](https://programminghistorian.org/lessons/building-static-sites-with-jekyll-github-pages#installing-dependencies-) and set up a Jekyll site locally.

*Jul 23, 2017:* I started [running Jekyll locally with Docker](https://kristofclaes.github.io/2016/06/19/running-jekyll-locally-with-docker/) and took great satisfaction in [uninstalling all gems](https://stackoverflow.com/questions/8095209/uninstall-all-installed-gems-in-osx) from my local environment.

## Picking a theme

*Day 8:* The boilerplate site Jekyll generates is fairly simplistic so I searched existing [Jekyll themes](https://jekyllthemes.io/) for a better starting point.

Over the course of the next few days (and months) I explored various options:

- [Clean Blog](https://startbootstrap.com/template-overviews/clean-blog/) with some modifications
- *Day 14:* [Beautiful Jekyll](http://deanattali.com/beautiful-jekyll/) with fewer modifications
- *Feb 18, 2017:* Building my own theme based on [Poole](http://getpoole.com/) and [Jekyll Now](http://www.jekyllnow.com/)
- *Sep 6, 2017:* [Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/) - my current theme, with *minimal* modifications (so far!)

## Font selection

*Day 19:* Not quite satisfied with the default fonts, I got caught in a Google wormhole reading up on optimal font selection for web sites. From my research I drew the following conclusions:

- Ensure a [body font size of at least 16px](https://www.smashingmagazine.com/2011/10/16-pixels-body-copy-anything-less-costly-mistake/) for readability
- Use a [system font stack](https://bitsofco.de/the-new-system-font-stack/) for fast loading fonts and a native feel. Doing this means my site renders in Apple's beautiful [San Francisco font](https://developer.apple.com/fonts/) on Macs, and Microsoft's [Segoe UI](https://www.microsoft.com/typography/fonts/family.aspx?FID=331) on Windows.

## Writing the first post

*Day 20:* At this stage I began to realise how long I was taking so started taking notes for a post on how to take an excessively long time to set up a blog. I set myself a deadline to push the site live in ten days so that I would 1) actually get it done, and 2) have a nice title for this post.

*Day 30:* After days of procrastination and ending up in more Google wormholes, my first post and the site were finally published.

It's great to finally have this blog up and running. Now to work out how many keystrokes it took...
