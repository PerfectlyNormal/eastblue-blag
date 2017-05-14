---
layout: page
title: /now -- What I'm Working on Now
menutitle: nownownow
permalink: /now/
---

<div id="gr_custom_widget_1494711042">
  <div class="gr_custom_container_1494711042">
    <h2 class="gr_custom_header_1494711042">
      <a style="text-decoration: none;" href="https://www.goodreads.com/review/list/2268122-per-christian?shelf=currently-reading">Reading</a>
    </h2>

    <div id="gr_spinner">
      <img src="{{ site.url }}/blag/images/spinspin.svg">
    </div>
  </div>
</div>
<script defer async type="text/javascript" charset="utf-8">
  var dataSource = 'https://eastblue.org/~perchr/goodreads.php';

  var parseHTML = function(str) {
    var tmp = document.implementation.createHTMLDocument();
    tmp.body.innerHTML = str;
    return tmp.body.children;
  };

  var request = new XMLHttpRequest();
  request.open('GET', dataSource, true);

  request.onload = function() {
    if (request.status >= 200 && request.status < 400) {
      var data = request.responseText;
      var doc = parseHTML(data)[0];

      var books = doc.getElementsByClassName("gr_custom_each_container_1494711042");
      var container = document.getElementsByClassName("gr_custom_container_1494711042")[0];
      while(books.length > 0) {
        container.appendChild(books[0]);
      }
      var spin = document.getElementById('gr_spinner');
      spin.parentNode.removeChild(spin);
      document.getElementsByClassName("post-content")[0].classList.add('with-goodreads');
    } else {
      var container = document.getElementById('gr_custom_widget_1494711042');
      container.parentNode.removeChild(container);
    }
  };

  request.onerror = function() {
    var container = document.getElementById('gr_custom_widget_1494711042');
    container.parentNode.removeChild(container);
  };

  request.send();
</script>

> It’s a nice reminder for myself, when I’m feeling unfocused. A public declaration of priorities.
>
> -- <cite>Derek Sivers</cite>

After one and half years of [learning carpentry]({{site.baseurl}}{% link _posts/2015-10-05-master-carpenter.md %}), I'm putting down my hammer, leaving it for the weekends, and [going back to hammering on a keyboard full-time]({{site.baseurl}}{% link _posts/2017-03-17-keyboard-banger.md %}) again. Looking forward to getting started.

Busy exploring new (to me) technologies so I'm not totally hopeless when I start working. Angular and C&#9839; and .NET and everything I've not used at all, or in a long while.

Still busy getting the stable and garage finished and ready for use. Most of the troublesome things are done, and now it's just the last 20% taking another 80% of the time to finish.

Just agreed to help the neighbors add an additional bedroom to their house, which will be good training for when we can afford (and have a need) to do something similar with our house. They're way more confident in my skills than I am…

Dove back into the [Malazan Book of the Fallen](https://en.wikipedia.org/wiki/Malazan_Book_of_the_Fallen) for a re-read. Understanding a lot more and recognizing people without having their names spelled out this time around. Amazing how much I "missed" the first time around. Also planning on all the novellas, prequels and related books.

Finished [Iron Fist](http://www.imdb.com/title/tt3322310), which was watchable, but not enjoyable. Continued over to [Legion](http://www.imdb.com/title/tt5114356/) and it's looking better so far, two episodes in.

<hr class="divider">

<p class="post-meta">
  If my activities change, I'll update this page. Last update was in <strong>April 2017</strong>.
</p>