---
layout: pg_root
permalink: /writing/
root:
  name: writing
  url: /writing
yahWrt: true
splash: /assets/img/splash_470x80_books.png
---

# The Library

{% for library in site.libraries %}
<div class="librarybox">
  <a href="/writing/{{ library.library }}/">
  <p class="libname">{{ library.name }}</p>
  <p class="libdesc">{{ library.desc | downcase }}</p>
  <div class="libicon">
    {% assign icon = 'svg/' | append: library.icon %}{% include {{ icon }} %}
  </div>
  </a>
</div>
{% endfor %}

{% assign libpages = site.writing | where: "library",nil | sort: "date" | reverse %}
{% if libpages.size > 0 %}
* * * * *

## Loose Writings

<ul class="postlist">
  {% for post in libpages %}
  <li><a href="{{ post.url }}">{{ post.title }}</a>
      <span class="postinfo">
      // <span class="date">{{post.date | date: "%F"}}</span>
      {% if post.tags != empty %}
      <ul class="tags">
        {% for tag in post.tags | sort %}
        <li><a href="/writing/{{ page.library }}/tags.html#{{ tag }}">{{ tag }}</a></li>
        {% endfor %}
      </ul>
      {% endif %}
      </span>
  {% endfor %}
</ul>
{% endif %}
