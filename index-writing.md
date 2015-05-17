---
title: writing
layout: index
yahLit: true
css: 
- /assets/lit/libindex.css
permalink: /writing/
---

<div class="latestposts">
  <h3>recent library pages</h3>
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

<div class="latestposts">
  <h3>recent blog posts</h3>
  <ul>
    {% for post in site.posts | limit: 3 %}
    <li><a class="postlink" href="{{post.url}}">{{post.title}}</a>
    <span class="postinfo"> // <span class="date">{{post.date | date: "%F"}}</span>
      {% if post.tags != empty %}
        <ul class="tags">
        {% for tag in post.tags %}
        <li><a href="/writing/blog/tags.html#{{tag}}">{{ tag }}</a></li>
        {% endfor %}
        </ul>
      {% endif %}
    </span>
    </li>
    {% endfor %}
  </ul>
</div>

<div class="liblist">
    <h1>The Library</h1>
    <ul>
      <li>
        <img src="/assets/lit/icon-sakura_x32.png" class="floatleft noborder" />
        <a class="liblink" href="/writing/blog/">shizuka hokura</a>
        <p class="libinfo">a small place for small thoughts</p>
      </li>
      {% assign libs = (site.data.libraries | sort) %}
      {% for lib in libs %}
      {% assign libid = lib|first %}
      {% assign libmeta = lib|last %}
      <li>
        <i class="fa fa-book"></i>
        <a class="liblink" href="/writing/{{libid}}/">{{libmeta.name}}</a>
        {% if libmeta.desc %}
          <p class="libinfo">{{libmeta.desc}}</p>
        {% endif %}
      {% endfor %}
    </ul>
</div>
