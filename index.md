---
layout: page
---
{% include JB/setup %}
<ul class="posts">
  {% for post in site.posts %}
    <li><span><h4>{{ post.date | date_to_string }}</h4></span><h4> &raquo;</h4><a href="{{ BASE_PATH }}{{ post.url }}"><h4>{{ post.title }}</h4></a></li>
  {% endfor %}
</ul>