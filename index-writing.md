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
  <h3>recently published</h3>
  <ul>
    {% assign pages = (site.writing | sort: "date" | reverse) %}
    {% for page in pages limit: 3 %}
    <li><a class="postlink" href="{{page.url}}">{{page.title}}</a>
      <span class="postinfo">
      // <span class="lib"><a href="/writing/{{page.library}}/">{{ site.data.libraries.[page.library].name }}</a></span>
        {% if page.book %}
        / <span class="book"><a href="/writing/{{page.library}}/#{{page.book}}">{{site.data.libraries.[page.library].books.[page.book].name}}</a></span>
        {% endif %}
      </span>
    </li>
    {% endfor %}
  </ul>
</div>

<div class="libindex">
  <section>
    <h2><a class="bloglink" href="/writing/blog/">Shizuka Hokura</a></h2>
    <div class="latestposts">
      <h3>recent posts</h3>
      <ul>
        {% for post in site.posts | limit: 3 %}
        <li><a class="postlink" href="{{post.url}}">{{post.title}}</a>
        <span class="postinfo"> // <span class="date">{{post.date | date: "%F"}}</span>
          {% if post.tags != empty %}
            <ul class="tags">
            {% for tag in post.tags %}
            <li><a href="/writing/blog/tags/#{{tag}}">{{ tag }}</a></li>
            {% endfor %}
            </ul>
          {% endif %}
        </span>
        </li>
        {% endfor %}
      </ul>
    </div>
  </section>
  <section>
    <h2>libraries</h2>
    <ul>
      {% for lib in site.data.libraries %}
      {% assign libid = lib|first %}
      {% assign libmeta = lib|last %}
      <li><a class="liblink" href="/writing/{{libid}}/">{{libmeta.name}}</a>
      {% endfor %}
    </ul>
  </section>
</div>
