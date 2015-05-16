---
title: writing
layout: index
yahLit: true
css: 
- /assets/lit/libindex.css
permalink: /writing/
---

# writings

<div class="latestposts">
  <img src="/assets/lit/icon-book_x64.png" class="floatright noborder" />
  <h3>recent pages</h3>
  {% assign pages = (site.writing | sort: "date") %}
  {% for page in pages reversed %}
  <li>{{page.url}} - {{page.date}}</li>
  {% endfor %}
</div>

<div class="libindex">
<section>
<h2>libraries</h2>
<ul>
<li><a class="liblink bloglink" href="/writing/blog/">Shizuka Hokura</a>
{% for lib in site.data.libraries %}
<li><a class="liblink" href="/writing/{{lib.id}}/">{{lib.name}}</a>
{% endfor %}
</ul>
</section>
</div>
