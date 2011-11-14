---
layout: static
title: Doodles
---
<ul>
{% for post in site.categories.doodles %}
<li><a href="{{ post.url }}">{{ post.title }}</a><abbr>{{post.date | date_to_string }}</abbr></li>
{% endfor %}
</ul>

