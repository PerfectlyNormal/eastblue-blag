---
layout: post
title: "Flexo revived"
date: 2008-10-12
categories: [Project, Flexo]
---

This is a continuation of the story of [Flexo](/blag{% post_url 2008-04-01-introducing-flexo %}).

Shortly after writing the last post, I lost interest, and abandoned work on Flexo. I have now revived it, and also published it on [GitHub](http://github.com/), as all cool Ruby-developers do. (this one can be found under [PerfectlyNormal/Flexo](http://github.com/PerfectlyNormal/flexo).)

The big changes since last time is that the plugins actually work now. Also fixed up some missing require's that seemed to get included automatically on the Linux-box, but not on the Mac.

So it's now possible, although a bit clumsy, to install Flexo and get it up and running. The rest of the work will be to write plugins allowing it to actually do anything. And to do that, documentation is useful. Meaning I should get the website up and running, and add some more comments on the inner workings on Flexo so it's possible to create a RDoc that actually does anything.

And maybe create and publish a gem.
