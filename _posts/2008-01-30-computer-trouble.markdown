---
layout: post
title: "Computer Trouble"
date: 2008-01-30
categories: [Computers]
---

Did an _emerge -uD world_ here the other day. Among the many packages that was updated, _nvidia-drivers_ lurked. And I instantly felt nervous. Updating those binary drivers usually end up bad. Considered masking it, since the new features weren't anything I really wanted, and therefore, I could put off updating.
But then again, they could have improved something and just not told anybody about it. So I decided to take the chance.

The upgrade went smoothly, as one would expect. And I breathed a sigh of relief, and went back to my regular business.

Two days passed, and suddenly wine started segfaulted when trying to run anything. The application I wanted was command-line (and windows-only), which is why it took me a while to link this with the recent upgrade of my _nvidia-drivers_. But they were the ones to blame. Found the problem. Now, how to solve it?

Normally, I would just switch over to a console, shut down the display manager, rmmod and modprobe. But things get complicated. New drivers failed to detect I2C or something like that. So that did *not* work. Hurray, stuck in the CLI!

After that failed attempt, I recompiled my kernel, rebooted, re-emerged _nvidia-drivers_, and behold! Working graphics again. It's annoying things like these that make me wonder why I insist on running Linux on my computer. There's so much *work* just to keep things running. And I'm not entirely happy with things as they are now.

Then I touch a computer running the horrible abomination that is Windows, and I remember. And whenever I check my bank account, I remember why I'm not running Mac OS X.

There's still that upgrade to KDE4 as well. Really want to try it, but not sure if I am ready to fight with my computer for ages. Might just wait until they release 4.1. Or finally move back to Fluxbox. Or try out one of the other minimal window managers.

There's just too many choices :/
