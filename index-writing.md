---
title: writing
layout: index
permalink: /writing/
---

# writings

<pre><code>
{% for cat in site.categories %}
CAT "{{ cat | first }}"
{% for post in cat[1] %}
  POST {{post.url}} - {{post.title}}
{% endfor %}
{% endfor %}
</code></pre>