---
layout: default
---
<h1>Latest Posts</h1>

<ul>
  {% for post in site.posts %}
    <li>
      <h5><a href="{{ post.url }}">{{ post.title }}</a></h5>
      <p class="post_date">{{ post.date | date: "%-d %B %Y" }}</p>
      {{ post.excerpt }}
    </li>
  {% endfor %}
</ul>
