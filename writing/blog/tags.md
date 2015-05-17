---
title: Tags
parents:
- url: /writing/blog/
  title: blog
css:
- /assets/lit/blog.css
---

# tags

<div class="tagindex">
  {% assign tags = (site.data.tags | sort) %}
  {% for tag in tags %}
    {% assign tagmeta = tag|last %}
    <h2 class="tagname">{{tag|first}}</h2>
    {% if tagmeta.desc %}
    <p class="tagdesc">{{tagmeta.desc}}</p>
    {% endif %}
    
  {% endfor %}
</div>