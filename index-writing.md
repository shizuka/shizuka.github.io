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
<div class="librarybox" id="{{ library.library }}">
  <a href="/writing/{{ library.library }}/">
  <div class="libicon">
    {% assign icon = 'svg/' | append: library.icon | append: '.svg' %}{% include {{ icon }} %}
  </div>
  <div class="libinfo">
    <p class="libname">{{ library.name }}</p>
    <p class="libdesc">{{ library.desc | downcase }}</p>
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
      {% include meta/postinfo.html %}
  {% endfor %}
</ul>
{% endif %}
