---
layout: post
title: "Revival and Matroska"
date: 2008-07-06
categories: [Project, Matroska]
---

Been ages since I last posted anything here. Just kind of lost interest, and got busy with other projects (as it usually is).

One of the things I've been working on, is [ruby-matroska](http://rubyforge.org/projects/matroska/). A gem designed primarily for working with [ordered chapters](http://www.uppcon.se/thefluff/hurfdurf/?p=8) in [matroska](http://www.matroska.org/) files.

The reason I started was that my media player of choice, [mplayer](http://www.mplayerhq.hu), currently doesn't support ordered chapters and all that nifty goodness. And since my anime is slowly adapting it, and that [we](http://eastblue.org/rye/) decided to use it for our [Death Note](http://en.wikipedia.org/wiki/Death_Note) releases, actually being able to use it was kind of important.

ruby-matroska consists of two binaries, of which only one is complete.

* _playlist_ takes a Matroska-file as an argument, analyzes it, and creates a mplayer compatible playlist, or launches mplayer with the right files.
* _unorder_, which is incomplete, takes a directory with ordered chapters, analyzes and creates self-contained matroska files, containing all the linked chapters in a file of their own. I can't see myself actually using the last one, but since I'm releasing this gem upon the world, I thought it would be a nice feature to have.

It's also open source, and the source is available through [my github repository](http://github.com/PerfectlyNormal/ruby-matroska/tree/master). Also got a [bugtracker](http://perfectlynormal.lighthouseapp.com/projects/13515-ruby-matroska/) over on [Lighthouse](http://www.lighthouseapp.com), but since I'm using the free version, I'm not really sure if it's accessible for others.

I'm currently relying on the user having [mkvtoolnix](http://www.bunkus.org/videotools/mkvtoolnix/) installed, but in the future, I would like to extend this project to also include a Matroska-parser. This would not only make it do it's thing a lot slower, but also be a fun lesson in implementing others formats.

Besides from removing dependencies, and making sure my SUIDCache-class is needed, I'm not really sure what practical use this library could have. And since I'm unable to find a completed [EBML](http://en.wikipedia.org/wiki/Extensible_Binary_Meta_Language) or matroska-library written for Ruby, I'm guessing not many others have seen a need either. (Or my Google-fu is just getting weaker)
