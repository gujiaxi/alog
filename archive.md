---
layout: page
title: Archive
permalink: /archive/
---

{% assign posts_by_year = site.posts | group_by_exp: "post", "post.date | date: '%Y'" %}
{% for year in posts_by_year %}
<h2 class="index-year">{{ year.name }}</h2>
<ul class="post-index date-first">
{% for post in year.items %}
<li>
  <time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%m-%d" }}</time>
  <a class="post-index-title" href="{{ post.url | relative_url }}">{{ post.title | escape }}</a>
  <span class="archive-tags">{% for tag in post.tags %}<a href="{{ '/tags/' | relative_url }}#{{ tag | slugify }}">{{ tag }}</a>{% endfor %}</span>
</li>
{% endfor %}
</ul>
{% endfor %}
