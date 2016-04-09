---
# Home
layout: pg_home
yahHome: true
splash: /assets/img/splash_470x240_minnehaha.png
---

<div id="homeindex">
<h1>this vortal cord</h1>
<h2>recent writings</h2>
{% assign allposts = site.writing | sort: "date" | reverse %}
{% for post in allposts limit:3 %}
<section>
<h1><a href="{{ post.url }}">{{ post.title }}</a></h1>
{% include meta/postinfo.html post=post %}
{% include meta/postexcerpt.html post=post %}
</section>
{% endfor %}
</div>
