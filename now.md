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

Quite out of date, as is to be expected. More to come later.

Turns out the GoodReads widget broke. Will have to look into that.

<hr class="divider">

<p class="post-meta">
  If my activities change, I'll update this page. Last update was in <strong>February 2018</strong>.
</p>
