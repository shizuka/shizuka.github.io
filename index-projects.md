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

<div class="projindex">
{% for project in site.data.projects %}
 {% if project.url %}{% capture projurl %}{{ project.url }}.html{% endcapture %}
 {% else %}{% capture projurl %}{{ project.id }}.html{% endcapture %}
 {% endif %}
 <div id="{{ project.id }}" class="project">
 {% unless project.noicon %}
  <img alt="{{ project.id }} icon" src="icon-{{ project.id }}.png" class="projicon {% if project.noborder %}noborder{% endif %}">
 {% endunless %}
 <div class="projname">
  {% if project.nolink %}
    {{ project.name }}
  {% else %}
    <a href="{{ projurl }}">{{ project.name }}</a>
  {% endif %}
 </div>
 <div class="projdesc" markdown = "1">{{ project.desc }}</div>
 </div>
{% endfor %}
</div>
