---
layout: post
title: "Exporting TextPattern"
date: 2009-01-09
categories: [Blag]
---

And then I have migrated the old articles from TextPattern and made pretty Jekyll-posts of them. Also fixed things with [Disqus](http://disqus.com), so commenting should work nicely. It's JavaScript-based, which I don't like, but it's taken care of by others, which I *do* like. Since then I won't have to do anything to have them, and shouldn't have to worry about exploits and things. Hopefully. Think there's some other features as well.

The script I used for exporting was based on the WordPress-one included with Jekyll:

{% highlight ruby %}
require 'rubygems'
require 'sequel'
require 'fileutils'

# NOTE: This converter requires Sequel and the MySQL gems.

module Jekyll
  module TextPattern
    # Reads a MySQL database via Sequel
    # and creates a post file for each post.
    QUERY = "select * from textpattern"

    def self.process(dbname, user, pass, host = 'localhost')
      db = Sequel.mysql(dbname, :user     => user,
                                :password => pass,
                                :host     => host)

      FileUtils.mkdir_p "_posts"

      db[QUERY].each do |post|
        # Get required fields and construct Jekyll compatible name
        title = post[:Title]
        slug = post[:url_title]
        date = post[:Posted]
        content = post[:Body]

        name = [date.strftime("%Y-%m-%d"), slug].join('-') + ".textile"

        # Get the relevant fields as a hash, delete empty fields
        # and convert to YAML for the header
        data = {
           'layout' => 'post',
           'title' => title.to_s,
           'tags' => post[:Keywords].split(',')
         }.delete_if { |k,v| v.nil? || v == ''}.to_yaml

        # Write out the data and content to file
        File.open("_posts/#{name}", "w") do |f|
          f.puts data
          f.puts "---"
          f.puts content
        end
      end
    end
  end
end
{% endhighlight %}