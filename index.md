---
layout: page
---
{% include JB/setup %}
<ul class="posts">
  {% for post in site.posts %}
    <li><span><h1>{{ post.date | date_to_string }}</h1>></span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}"><h1>{{ post.title }}</h1></a></li>
  {% endfor %}
</ul>