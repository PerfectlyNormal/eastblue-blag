---
layout: post
title: "Introducing Flexo"
date: 2008-04-01
categories: [Project, Flexo]
---

Flexo is, in addition to being a character in [Futurama](http://www.imdb.com/title/tt0149460), an IRC bot hacked together by me. (He's also the first image to be featured in this blag) *update (2009-01-09): image got lost in the export, and no need for it*.

Written in Ruby, and very much based on [Flux](http://rubyforge.org/projects/flux/). In fact, some 80% of the code is probably identical. The difference between Flexo and Flux is that Flux was written as a framework for creating IRC bots, while Flexo is written to be a single, stand-alone, extensible bot.

The base of Flexo is pretty much complete. Only problem is, the bot is pretty much useless as well. Right now it can connect to IRC, join channels, and idle. That's all it's supposed to do. The rest will be done using plugins.

Working slowly on a plugin for adding restricted triggers to Flexo, which will use a small database for storing users and their access levels. The restricted commands will in turn let me write a plugin for controlling Flexo directly through IRC. Additionally, I was planning on making a [DBus](http://www.freedesktop.org/wiki/Software/dbus) plugin as well, so that outside scripts can interface with a Flexo-instance easily.

After those plugins are done, it's finally time for my master plan. The main reason why Flexo was born. A [svn](http://subversion.tigris.org) notifier. I want it to inform a channel each time a commit to one of my svn repositories is made, with some basic information about it. I don't want everything to be public, so something like [CIA](http://cia.vc) is out of the question. Plus, writing my own is much more fun.

When it's done, it might even get released out into the wild. That's how much I like it.
