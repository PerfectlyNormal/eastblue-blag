---
layout: post
title: "Hooking up Gitorious"
date: 2009-01-16
categories: [Project, Rails]
---

I'm using a self-hosted instance of [Gitorious](http://gitorious.org) for all my not-quite-public projects, and have been very satisfied so far.

My only issue was that it was a bit harder than it should have been to fix up my Jekyll post-commit-hook. When I discovered that [GitHub](http://github.com) [publishes their hooks](http://github.com/pjhyett/github-services), there was no doubt in my heart that Gitorious needed something similar. So I set out to add that feature to Gitorious.

My [current fork](http://gitorious.org/projects/gitorious/repos/PerfectlyNormal-clone) has the hook-system working, but lacks a decent UI (or any UI at all) for adding hooks, so I currently have to setup everything manually. I'm using a slightly modified version of the hook-server, but everything else stays the same, so we can share hooks with GitHub and every system that's ready for dealing with GitHub's post-commit-hook can also receive calls from Gitorious, and everything should just work.

If I get time this weekend, I'll see if I can't hack up a jekyll-service, so I can move my post-receive-hook for this website into a common format, which would let me use it easier for multiple websites. And I'll try and get started on the UI. It'll probably end up somewhat similar to GitHub, but adapted to better match Gitorious's design.