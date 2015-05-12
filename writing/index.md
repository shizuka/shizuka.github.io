---
title: writing
layout: index
notapost: true
---

# writings


{% for cat in site.data.categories %}
<pre><code>
CAT {{cat.slug}} - {{cat.name}}
{% for page in (site.pages | where: "yahLit", true) %}{% if page.category == cat.slug %}
  PAGE {{page.path}} - {{page.title}}
{% endif %}{% endfor %}
</code></pre>