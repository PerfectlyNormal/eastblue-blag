---
layout: post
title: "Twitorious"
date: 2009-03-27
categories: [Project]
---

The last few days, in between work on [iEye](http://myieye.net) and school, I've extended my Twitter-script and made it into a full-fledged project. Named Twitorious. Which is slightly misleading, since that might suggest it only works on [Twitter](http://twitter.com), which is wrong.

It currently supports all microblogging-sites with a Twitter-compatible API, or at least it should, in theory. I've only tested it on Twitter and [Identi.ca](http://identi.ca).

Should be pretty easy to extend it with support for other sites. And notification methods, since it currently only supports [Growl](http://growl.info). The gem I use for Growl-support, [Meow](http://meow.rubyforge.org) only works on OS X, so that makes Twitorious OS X-only until someone writes a alternate notification-method. I don't really need anything else yet. I launch the update-command manually once in a while, and watches all the updates appear one after one.

If anyone is interested, I've put the [source up on GitHub](http://github.com/PerfectlyNormal/twitorious). There's a few things listed in [the TODO-file](http://github.com/PerfectlyNormal/twitorious/blob/3468a230252960fd0ccee1c8b71f0aad02030faf/TODO). I'll slowly work on a few of those as soon as I have time and I feel like I need it, but contributions are of course welcome.

Twitorious is [MIT-licensed](http://www.opensource.org/licenses/mit-license.html)