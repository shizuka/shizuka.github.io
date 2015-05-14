---
title: writing
layout: index
yahLit: true
css: 
- /assets/lit/catindex.css
permalink: /writing/
---

# writings

<div class="latestposts">
  <img src="/assets/lit/icon-book_x64.png" class="floatright noborder" />
  <h3>recent posts</h3>
  <ul>
    {% for post in (site.posts | limit: 5) %}
    <li>
      <p class="postinfo">
        <a class="postlink" href="{{ post.url }}">{{ post.title }}</a>
        <span class="postcat">{{ post.categories }}</span>
        <span class="posttags">{{ post.tags | join: "/" }}</span>
      </p>
    </li>
    {% endfor %}
  </ul>
</div>

<div class="catindex">
<section>
<h2>categories</h2>
<ul>
{% for cat in site.categories %}{% assign catname = cat[0] %}{% assign catpages = cat[1] %}
{% assign catmeta = site.data.categories.[catname] %}
<li>
<a class="catlink" href="/writing/{{catname}}/">{{catmeta.name}}</a>
</li>
{% endfor %}
</ul>
</section>
</div>
