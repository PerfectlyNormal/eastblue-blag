---
layout: post
title: "File uploads with Passenger"
date: 2009-05-18 10:54
comments: true
categories: [Rails, Note to Self, Hack]
---

Mostly just a quick note to myself.

As [noted](http://rack.lighthouseapp.com/projects/22435/tickets/46-rack-in-ruby-19-throws-fatal-error-on-small-uploads) [elsewhere](https://rails.lighthouseapp.com/projects/8994/tickets/2497-rack-in-rails-232-throws-fatal-error-with-small-uploads) on the web, there is some problems with [Rack](http://rack.rubyforge.org) and file uploads when using Ruby 1.9.

The problem has been [fixed](http://github.com/rack/rack/commit/44ed4640f077504a49b7f1cabf8d6ad7a13f6441) in Rack, but [Passenger](http://www.modrails.com) bundles their own Rack, which makes it a bit tricky until they release a newer version.

So I went into `/usr/lib/ruby19/gems/1.9.1/gems/passenger-2.2.2/vendor` and removed `rack-1.0.0-git`, replacing it with a fresh checkout from [GitHub](http://github.com/rack/rack/tree).

One `/etc/init.d/apache restart` later, and all is good.