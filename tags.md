---
layout: page
title: Tags
permalink: /tags/
---

<style>
.tag-section { margin-bottom: 2em; }
.tag-section h3 { margin-bottom: 0.3em; }
.tag-posts { list-style: none; margin: 0; padding: 0; }
.tag-posts li {
  display: flex;
  align-items: baseline;
  gap: 0.6em;
  padding: 0.1em 0;
}
.tag-posts .post-date { color: #aaa; font-size: 0.88em; white-space: nowrap; }
</style>

{% assign all_tags = site.posts | map: "tags" | join: "," | split: "," | uniq | sort %}
{% for tag in all_tags %}
<div class="tag-section" id="{{ tag | slugify }}">
<h3># {{ tag }}</h3>
<ul class="tag-posts">
{% for post in site.posts %}{% if post.tags contains tag %}
<li>
  <span class="post-date">{{ post.date | date: "%Y-%m-%d" }}</span>
  <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
</li>
{% endif %}{% endfor %}
</ul>
</div>
{% endfor %}
