---
layout: post
title: "Migration"
date: 2009-01-08
categories: [Blag]
---

After a day of hacking and toying around, I think I can finally say that my new blag is complete. Yet another one. This might last a bit longer than the others, but not too likely.

The biggest change now is that I've moved to yet another blogging tool. First it was [WordPress](http://wordpress.com), then it was Blag2 (a home-made Rails-app that is now rotting away in one of my many repositories), then I moved to [TextPattern](http://textpattern.com). Finally, the time has come to go back to good old static web pages.

I now have a blag powered by [Jekyll](http://github.com/mojombo/jekyll) and [Git](http://git-scm.com). Also some magic with a `post-commit`-hook and [rsync](http://samba.anu.edu.au/rsync/). And there's some [Pygments](http://pygments.org) involved for that fancy code highlighting.

The start of it all was [Dr Nic's article](http://drnicwilliams.com/2008/12/21/migrating-project-websites-to-github-pages-with-sake-tasks-new-websites-with-jekyll_generator) that introduced me to the wonderful Jekyll. This was just what I was looking for for [Flexo](http://perfectlynormal.github.io/flexo/) ([on GitHub](http://github.com/PerfectlyNormal/flexo/)). And while reading the documentation, I decided to recreate my PerfectlyNormalBlag using Jekyll.

I decided that I wanted to keep the same URL without any fancy redirections or other ways of getting content from GitHub, so I set out to fix my own repository and make it work the same way. I use [Gitorious](http://www.gitorious.org) for all my own repositories, and the first task was therefore to dive into the source and figure out how their hook system works.

Turns out all the repositories have a symlink to a common hooks-directory containing the hooks, which is okay as long as no one needs anything special. If that don't get fixed in the future, I might take a stab at it myself, since per-project hooks would be nice to be able to configure. Anyways, the first step was to remove the symlink and create a directory instead. In there I made a symlink to all the regular hooks, except for `post-receive`, which I placed a line executing the real script in.

{% highlight bash %}
#!/bin/bash

APPBASE="/usr/bin"
JEKYLL=$APPBASE/jekyll
RSYNC=$APPBASE/rsync
GIT=$APPBASE/git

JEKYLL_ARGS="--pygments"
SITE_DIR=/var/dev/gitorious/eastblue
RSYNC_DIR=$SITE_DIR/_site/
RSYNC_ARG="-r $RSYNC_DIR eastblue@www-host:www"

cd $SITE_DIR
unset GIT_DIR
$GIT pull
$JEKYLL $JEKYLL_ARGS
$RSYNC $RSYNC_DIR $RSYNC_ARG

exit 0
{% endhighlight %}

As you can see, all it does is pulling down the changes, running jekyll, and rsync them up to the web server. The big secret here is to unset `GIT_DIR`, or possibly just change it to the right folder.

With the `GIT_DIR`-trick figured out, the rest was just creating the layout and starting to write the content. I'm now typing in the excellent TextMate, which easily defeats any web-UI for a blag I've ever tried. And having everything included in my dev-server-backup is also very nice.

Figured I should try adding some commenting, and hopefully also making it possible to use the tags for anything later on, but I think this might be enough for one day. Should export my previous entries from TextPattern as well, which <del>shouldn't be too hard</del> [isn't hard]({% post_url /blag/2009-01-09-exporting-textpattern %}).