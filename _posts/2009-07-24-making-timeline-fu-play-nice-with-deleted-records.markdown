---
layout: post
title: "Making timeline_fu play nice with deleted records"
date: 2009-07-24
categories: [Rails]
---

We use [`timeline_fu`](http://github.com/giraffesoft/timeline_fu/tree/master) for a simple timeline in [iEye](https://ieye.org). The problem is that objects can be deleted, making the timeline events freak out a bit, since their subjects might not be there any longer.

One option is to make sure all timeline events referring to a deleted object is deleted as well, but that makes things disappear from the timeline.

Since we don't want things to suddenly disappear, I installed [`is_paranoid`](http://github.com/semanticart/is_paranoid/tree/master), so the objects still exist, they're just hidden by default.
Now, this didn't help much, since I couldn't find a easy way to decide which finder `timeline_fu` uses to get the objects. Which means it's time to start hacking things.

In my model, `TimelineEvent`, I now have this:

{% highlight ruby %}
def actor
  find_with_destroyed(actor_type, actor_id)
end

def subject
  find_with_destroyed(subject_type, subject_id)
end

def secondary_subject
  find_with_destroyed(secondary_subject_type, secondary_subject_id)
end

private

def find_with_destroyed(klass, id)
  return nil unless id
  obj = eval klass.classify
  res = obj.find_with_destroyed(id) rescue nil
  res = obj.find(id) unless res
  res
end
{% endhighlight %}

And everything works like a charm.