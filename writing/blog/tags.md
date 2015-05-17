---
title: Tags Index
parents:
- url: /writing/blog/
  title: blog
css:
- /assets/lit/blog.css
---

# posts by tag
{:.bigtitle}

<div class="tagindex">
  {% capture tags %}
    {% for tag in site.tags %}
      {{ tag[0] }}
    {% endfor %}
  {% endcapture %}
  {% assign sortedtags = tags | split:' ' | sort %}
  {% for tag in sortedtags %}
  <div class="tagbox">
    {% assign tagmeta = site.data.tags.[tag] %}
    <div class="taginfo">
      <h2 id="{{tag}}" class="tagname">{{tag}}</h2>
      {% if tagmeta.desc %}
      <p class="tagdesc">{{tagmeta.desc}}</p>
      {% endif %}
    </div>
    <ul class="taglist">
    {% for post in site.tags[tag] %}
      <li>
        <a class="postlink" href="{{post.url}}">{{post.title}}</a>
        <span class="postinfo">
          // <span class="date">{{post.date | date: "%F"}}</span>
          {% capture posttags %}{% for posttag in post.tags %}{% if posttag == tag %}{% continue %}{% endif %}<li><a href="#{{posttag}}">{{ posttag }}</a></li>{% endfor %}{% endcapture %}
          {% if posttags != empty %}
            <ul class="tags">
            {{posttags}}
            </ul>
          {% endif %}
        </span>
      </li>
    {% endfor %}
    </ul>
  </div>
  {% endfor %} <!-- for tag in tags -->
</div>