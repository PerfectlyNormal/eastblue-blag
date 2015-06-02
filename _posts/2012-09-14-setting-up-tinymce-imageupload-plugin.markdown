---
layout: post
title: "Setting up TinyMCE ImageUpload plugin"
date: 2012-09-14 14:04
comments: true
categories: Rails
published: false
---

FIXME: Write something clever

## Quick guide to setting up tinymce-rails-imageupload

### Installing

``` ruby Gemfile
gem 'tinymce-rails-imageupload'
```

`tinymce-rails-imageupload`, as the gem is called, automatically pulls in a version of [tinymce-rails](1) that is compatible with the gem, so no need to specify that as well. I have it outside of the `assets`-group because of the way `TinyMCE` loads plugins (it doesn't play nice with precompiling anyways).

### Routing

