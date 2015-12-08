---
title: Shizuka Hokura
layout: blogindex
yahLit: true
css:
- /assets/lit/blog.css
permalink: /writing/blog/
---

![sakura flower](/assets/lit/icon-sakura_x160.png){: .noborder}
{: .floatright}

# shizuka hokura
{: .bigtitle}

a small place for small thoughts
{: style="font-size: smaller"}

<div class="postindex">
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
</div>