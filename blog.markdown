---
layout: static
title: Blog
---
<ul>
{% for post in site.categories.blog %}
<li><a href="{{ post.url }}">{{ post.title }}</a><abbr>{{post.date | date_to_string }}</abbr></li>
{% endfor %}
</ul>

