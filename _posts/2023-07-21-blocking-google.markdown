---
layout: post
title: "Blocking Google"
date: 2023-07-21 19:08
comments: false
categories: Blag
---

Joining [several](http://joeyh.name/blog/entry/become_ungoogleable/)
[others](https://vasilis.nl/nerd/how-to-disagree-with-googles-privacy-policy/)
[recently](https://adactio.com/journal/20315), I've decided that I don't really
want Google around anymore.

I've been avoiding their products as much as possible for a long while, but
that's not really enough. So I'm also updating the `robots.txt` for all domains
I control to exclude Googlebot.

```
User-agent: Googlebot 
Disallow: /
```

Those sites are very small, with close to no traffic, so it doesn't really
matter, nor will they notice in any way. But change is possible, even if it
starts small.

Had to log in to my registrar and check which domains I actually own, which
made me chuckle a bit. I have a tendency to buy a domain for some kind of
side-project that ends up going nowhere and leaving a placeholder-site around.

Would be nice if they came to their senses and stopped being evil, but not
holding my breath for that one.
