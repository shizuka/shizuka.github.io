---
title: writing
layout: index
css: 
- /assets/lit/catindex.css
permalink: /writing/
---

# writings

<div class="latestposts">
  <h2 id="latestposts">Recent Pages</h2>
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
{% for cat in site.categories %}{% assign catname = cat[0] %}{% assign catpages = cat[1] %}
{% assign catmeta = site.data.categories.[catname] %}
<section class="category">
## {{catmeta.name}}
<ul>
{% for page in catpages %}<li><a href="{{page.url}}">{{page.title}}</a></li>{% endfor %}
</ul>
</section>
{% endfor %}
</div>