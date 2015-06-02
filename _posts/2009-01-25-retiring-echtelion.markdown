---
layout: post
title: "Retiring echtelion"
date: 2009-01-25
categories: [Computers, nostalgia]
---

Finally, I think the time has come to put my old, trusty server `echtelion` to rest. Although the hardware has been slightly changed over the years, and the names switched time and time again, `echtelion` is still the first server I ever got up and running with Linux.

Starting out with [Slackware](http://www.slackware.com), and ending up with [Gentoo](http://www.gentoo.org) for the last two years or so, `echtelion` has been with me all this time. Faithfully providing file sharing via Samba, web pages with Apache, mail with qmail, IRC with irssi, databases with MySQL and PostgreSQL, and plenty of other things.

But the file sharing has been taken over by my file server, `anduin`.

I don't really host any web pages any longer, the only things are taken care of by the development server, `maeglin`, and [eastblue.org](http://eastblue.org/), using a shared webhost.

I've already ended up with a MySQL instance on `maeglin`, so I can just move all the databases over there.

And as mentioned in "[Summer](/blag{% post_url 2008-05-29-summer %})", I've moved over to Google Apps for my email.

Which only leaves IRC. And frankly, that's not worth a dedicated machine.

I still have Apache running, faithfully serving up things for [northblue.org](http://northblue.org), but that is so empty that I could easily move it over to `maeglin`, which would also let me drop [Pound](http://www.apsis.ch/pound/) from the firewall and just send everything over to `maeglin`.

Since the file server is meant to accompany me whenever I go to a LAN party or something like that, I'm a bit reluctant to have things other than Samba and NFS running there, so it looks like `maeglin` will be my IRC box as well from now on.

Another thing that this simplifies, is SSH. I usually want to end up on the IRC box when logging in, so I have the SSH port forwarded there. But it also happens that I want to push some changes to a repository when I'm on the outside, so I have to use different ports. No more!