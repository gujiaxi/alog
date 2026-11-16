---
layout: page
title: Tags
permalink: /tags/
---

{% assign all_tags = site.posts | map: "tags" | join: "," | split: "," | uniq | sort %}
{% for tag in all_tags %}
<div class="tag-section" id="{{ tag | slugify }}">
<h2 class="index-year"># {{ tag }}</h2>
<ul class="post-index date-first">
{% for post in site.posts %}{% if post.tags contains tag %}
<li>
  <time class="post-index-date" datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%Y-%m-%d" }}</time>
  <a class="post-index-title" href="{{ post.url | relative_url }}">{{ post.title | escape }}</a>
</li>
{% endif %}{% endfor %}
</ul>
</div>
{% endfor %}
