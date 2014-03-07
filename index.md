---
layout: page
title: 10io
tagline: Crafting bytes on Internet.
---

{% for post in site.posts limit:5 %}
  <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
  {{ post.excerpt }}
  <hr />
{% endfor %}
