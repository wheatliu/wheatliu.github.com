---
layout: page
---
{% include JB/setup %}
<ul class="posts">
  {% for post in site.posts %}
    <li><span style="font-size:18px;">{{ post.date | date_to_string }}</span> &raquo;<a href="{{ BASE_PATH }}{{ post.url }}" style="font-size:18px;">{{ post.title }}</a></li>
  {% endfor %}
</ul>