---
title: Shizuka Hokura
layout: index
yahLit: true
css:
- /assets/lit/libindex.css
permalink: /writing/blog/
---

# shizuka hokura

<div class="libindex">
<ul>
{% for post in site.posts %}
<li><a href="{{post.url}}" class="postlink">{{post.title}}</a> // {{post.date | date: "%F"}}</li>
{% endfor %}
</ul>
</div>