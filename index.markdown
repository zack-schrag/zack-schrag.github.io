---
layout: default
title: Blog
---
<ul>
  {% for post in site.posts %}
  <h5><a href="{{ post.url }}">{{ post.title }}</a></h5>
  {% endfor %}
</ul>
