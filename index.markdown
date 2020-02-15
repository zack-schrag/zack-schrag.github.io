---
layout: default
---
<h1>Latest Posts</h1>

<ul>
  {% for post in site.posts %}
    <li>
      <h4><a href="{{ post.url }}">{{ post.title }}</a></h4>
      {{ post.excerpt }}
    </li>
  {% endfor %}
</ul>
