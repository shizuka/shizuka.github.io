---
title: Shizuka Hokura
permalink: /writing/blog/
layout: pg_subpage
splash: /assets/img/splash_470x80_journal.png
root:
  name: writing
  url: /writing
yahWrt: true
---

# shizuka hokura

> a small place for small thoughts
{:.noitalic}

<ul>
  {% for post in site.posts %}
  <li>
    <a class="postlink" href="{{post.url}}">{{post.title}}</a>
    <span class="postinfo">
      // <span class="date">{{post.date | date: "%F"}}</span>
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