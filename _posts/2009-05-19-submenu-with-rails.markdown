---
layout: post
title: "Submenu with Rails"
date: 2009-05-19 10:54
comments: true
categories: Rails
---

Quick little trick I use to render a submenu for each view in the controller, without having to specify anything

{% highlight ruby %}
class ApplicationController < ActionController::Base
  before_filter :render_submenu

  def render_submenu
    controller = self.class.to_s.tableize.split("_")[0]
    @submenu   = render_to_string :partial => "#{controller}/submenu" rescue nil
  end

  ...
end
{% endhighlight %}

And then somewhere in my application layout

{% highlight erb %}
<% if @submenu || @content_for_submenu %>
  <div id='submenu'>
    <ul id='submenu-list'>
      <% if @submenu             %><%= @submenu %><%end%>
      <% if @submenu && @content_for_submenu %><li class="separator" id="this-page-separator">&nbsp;</li><% end %>
      <% if @content_for_submenu %><%= @content_for_submenu %><% end %>
    </ul>
  </div>
<% end %>
{% endhighlight %}

This lets each individual view add some links to their submenu with a simple `<% content_for :submenu do %>` block, and at the same time, render a submenu for the entire controller, placed in `app/controllers/mycontroller/_submenu.html.erb`