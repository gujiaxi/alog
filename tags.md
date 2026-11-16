---
layout: page
title: Tags
permalink: /tags/
---

{% assign tags = site.tags | sort %}
{% for tag in tags %}
<h2 class="index-year" id="{{ tag[0] | slugify }}"># {{ tag[0] }}</h2>
<ul class="post-index date-first">
{% for post in tag[1] %}
<li>
  <time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%Y-%m-%d" }}</time>
  <a class="post-index-title" href="{{ post.url | relative_url }}">{{ post.title | escape }}</a>
</li>
{% endfor %}
</ul>
{% endfor %}
