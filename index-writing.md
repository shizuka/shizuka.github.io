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
  <h2><a href="/writing/blog/">Shizuka Hokura</a></h2>
  <h3>recent posts</h3>
  <ul>
    {% for post in (site.posts | limit: 3) %}
    <li>
      <p class="posthead">
        <a class="postlink" href="{{ post.url }}">{{ post.title }}</a> // <span class="postdate">{{ post.date }}</span>
      </p>
      <p class="postinfo">
        in categories | tags, tags, tags
      </p>
    </li>
    {% endfor %}
  </ul>
</div>

<div class="libindex">
<section>
<h2>libraries</h2>
<ul>
{% for lib in site.data.libraries %}
<li><a class="liblink" href="/writing/{{lib.id}}/">{{lib.name}}</a>
{% endfor %}
</ul>
</section>
</div>
