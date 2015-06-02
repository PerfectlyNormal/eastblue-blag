---
layout: post
title: "Delaying Paperclip"
date: 2009-05-07 10:54
comments: true
categories: [Rails, Paperclip, delayed_job]
---

I'm hard at work on [iEye](https://ieye.org) these days.

To take care of images, we use [Paperclip](http://thoughtbot.com/projects/paperclip/), and are loving it. But according to [New Relic's RPM](http://www.newrelic.com/), the slowest part is when we upload a image, and it creates a few different versions (not that it was any surprise that that was slow). I searched a bit around for a easy way to make Paperclip work with [delayed\_job](http://tobi.github.com/delayed_job), but soon concluded that I would have to do it myself. Which I did.

#### ImageJob

The first step was to create a small wrapper-class for my model. I'm attaching images to a `Image`-model, so this new class was called `ImageJob` (and lives in `lib/`, since it's not really a model).

(This is of course not the first step, but I image that getting delayed\_job installed and ready is not a problem.)

{% highlight ruby %}
class ImageJob < Struct.new(:image_id)
  def perform
    Image.find(self.image_id).perform
  end
end
{% endhighlight %}

I tried to just enqueue the `Image` itself, but that gave me some errors when de-serializing the object from the database. And we don't really need to store the entire model in the database when we can just fetch it when we need it.

#### Processing

Then I added a column to the images-table, letting me know if it was still processing the image.

{% highlight ruby %}
class AddProcessingToImage < ActiveRecord::Migration
  def self.up
    add_column :images, :processing, :boolean, :default => true
  end

  def self.down
    remove_column :images, :processing
  end
end
{% endhighlight %}

It defaults to `true`, since Rails refused to set it to `true` before Paperclip started doing its thing. Might have been solved with some more investigation, but this worked quickly. This means that for an existing site, you would have to create a job for each existing image, and let them be processed, or go change each existing image to be done.

#### Stopping Paperclip

Then it was time to change the `Image` creation process, so we halted Paperclip if it was just created.

So in the model (`Image` in this case), I hooked into Paperclip, and stopped it. Then added a job for it to be processed later.

{% highlight ruby %}
before_attachment_post_process do |attachment|
  # Stop any further processing by Paperclip
  false if attachment.processing?
end
after_create do |image|
  Delayed::Job.enqueue ImageJob.new(image.id)
end
{% endhighlight %}

#### Performing the resizing

{% highlight ruby %}
# Force Paperclip to reprocess the image, and
# since we set processing to false before,
# it will manage to generate thumbnails on this attempt.
def perform
  self.processing = false
  self.attachment.reprocess!
  self.save
end
{% endhighlight %}

#### Default images when processing

Since Paperclip doesn't really know (or care) that we stopped it from creating images, it still returns the URLs for images that don't exist. And we don't want to serve non-existing images to visitors.

So instead of calling `@image.attachment.url(:thumbnail)` in my views, I added a little helper method to the `Image`-model with the same syntax, but returns the URL for a placeholder image if this image is still processing.

{% highlight ruby %}
# Convenience method to avoid calling @image.attachment.url in views,
# and also makes it possible to do a little dirty hack, returning
# different URL for images that are pending
def url(style = :original)
  if self.image && processing? && style != :original
    return attachment.send(:interpolate, @@default_url, "pending_#{style}")
  end
  attachment.url(style)
end
{% endhighlight %}

The default URL we use is stored as `@@default_url = "/system/:class/missing/:style.:locale.png"`, and gives URLs like `/system/images/missing/pending_thumbnail.en.png` when we are waiting for the thumbnail.

#### Running the worker

To actually get the workers running, and the jobs done, I use [God](http://github.com/mojombo/god) to make sure it keeps running, and a configuration based on [what GitHub uses](http://github.com/blog/229-dj-god)

{% highlight ruby %}
ENV['RAILS_ENV']  ||= "staging"
ENV['RAILS_ROOT'] ||= "/var/www/iEye/beta/current"
ENV['GOD_UID']    ||= "ieye"
ENV['GOD_GID']    ||= "users"

God.pid_file_directory = "#{ENV['RAILS_ROOT']}/log"

2.times do |num|
  God.watch do |w|
    w.name     = "ieye-beta-dj-#{num}"
    w.group    = 'ieye-beta-dj'
    w.uid      = ENV['GOD_UID']
    w.gid      = ENV['GOD_GID']
    w.interval = 30.seconds
    w.start    = "rake -f #{ENV['RAILS_ROOT']}/Rakefile jobs:work RAILS_ENV=#{ENV['RAILS_ENV']}"
    w.stop     = "kill -9 `cat #{w.pid_file}`"

    # restart if memory gets too high
    w.transition(:up, :restart) do |on|
      on.condition(:memory_usage) do |c|
        c.above = 300.megabytes
        c.times = 2
      end
    end

    # determine the state on startup
    w.transition(:init, { true => :up, false => :start }) do |on|
      on.condition(:process_running) do |c|
        c.running = true
      end
    end

    # determine when process has finished starting
    w.transition([:start, :restart], :up) do |on|
      on.condition(:process_running) do |c|
        c.running = true
        c.interval = 5.seconds
      end

      # failsafe
      on.condition(:tries) do |c|
        c.times = 5
        c.transition = :start
        c.interval = 5.seconds
      end
    end

    # start if process is not running
    w.transition(:up, :start) do |on|
      on.condition(:process_running) do |c|
        c.running = false
      end
    end
  end
end
{% endhighlight %}

This starts two job runners, and keeps them running. It's important to restart them after each deploy, so in my `deploy.rb`, right before I `touch tmp/restart.txt`, I also have `sudo god restart ieye-beta-dj` to take care of that. Might also be wise to have god start after a reboot, as I found out after researching why things weren't being run.