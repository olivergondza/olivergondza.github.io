---
layout: main
---

{% for post in site.posts %}
  <h2><a href=".{{ post.url }}">{{ post.title }}</a></h2>
  <p><small>{{ post.date | date: "%Y-%m-%d" }}</small><small style="float: right">Post by {{ post.author }}</small><div style="clear: both;"></div></p>
  <p style="color: #555; font-size: small;">{{ post.excerpt | remove: '<p>' | remove: '</p>' }}</p>
  <hr style="border-width: 0 0 1px 0; :border-color: #fff;">
{% endfor %}
