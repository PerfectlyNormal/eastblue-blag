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

Been one and a half month in [my new job](https://iosoft.se), and it's going great so far. My step count has plummeted though, averaging 4-5k each day instead of the 14-15k that were usual when doing carpentry all day.

ASP.NET and Angular are a strange breed. So different from what I'm used to. But I'm getting there.

Still busy getting the stable and garage finished and ready for use. Most of the troublesome things are done, and now it's just the last 20% taking another 80% of the time to finish. Also started inside, closing up the doorway into a room, adding a door and some shelves. Planning to move the TV and related toys out into the previous "library".

Still reading the [Malazan Book of the Fallen](https://en.wikipedia.org/wiki/Malazan_Book_of_the_Fallen). Also planning on all the novellas, prequels and related books. Got distracted by the [Red Rising trilogy](http://www.pierce-brown.com/red-rising-trilogy.html) for a bit. And the new [Assassin's Fate](http://www.robinhobb.com/2017/05/us-book-launch-today/) is still waiting. Looking forward to the vacation so I can finally do some real reading!

Hacked together a cleaner version of the GoodReads widget and put it in a sidebar here. Not very complicated, but had to create a [small proxy script](https://gist.github.com/PerfectlyNormal/87b3d8e03dbdb555f663a961266776be) to fetch data from GoodReads to avoid CORS issues.

[Legion](http://www.imdb.com/title/tt5114356/) was not to my wife's liking, so it's a solo-show from now on. Can't seem to find the time to watch it though. Second season of [Fortitude](http://www.imdb.com/title/tt3498622/?ref_=nv_sr_1) is the current show.

<hr class="divider">

<p class="post-meta">
  If my activities change, I'll update this page. Last update was in <strong>May 2017</strong>.
</p>