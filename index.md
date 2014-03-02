---
layout: page
title: 10io
tagline: Crafting bytes on Internet.
---
# My latest thoughts

{% for post in site.posts limit:2 %}
  <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
  {{ post.content }}
  <hr />
{% endfor %}