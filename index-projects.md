---
permalink: /projects/
layout: pg_root
root:
  name: projects
  url: /projects
yahPrj: true
splash: /assets/img/splash_470x80_code.png
---

# projects

<div class="sectionboxes">
{% for project in site.data.projects %}
<section markdown="1">
  {% unless project.noimage %}
![{{ project.name }}](icon-{{ project.id }}.png){% unless project.yesborder %}{: .noborder}{% endunless %}
  {: .floatright}
  {% endunless %}
  {% if project.nolink %}
## {{ project.name }} {#{{ project.id }}}
  {% elsif project.url %}
## [{{ project.name }}]({{ project.url }}) {#{{ project.id }}}
  {% else %}
## [{{ project.name }}]({{ project.id }}.html) {#{{ project.id }}}
  {% endif %}
{{ project.blurb }}
</section>
{% endfor %}
</div>