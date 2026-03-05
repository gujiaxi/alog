---
layout: page
title: Archive
permalink: /archive/
---

<style>
.archive-list { list-style: none; margin: 0; padding: 0; }
.archive-list li {
  display: flex;
  align-items: baseline;
  gap: 0.6em;
  padding: 0.15em 0;
}
.archive-date { color: #aaa; font-size: 0.88em; white-space: nowrap; flex-shrink: 0; }
.archive-title { flex: 1; min-width: 0; }
.archive-tags { margin-left: auto; flex-shrink: 0; text-align: right; }
.archive-tags a {
  display: inline-block;
  font-size: 0.78em;
  color: #888;
  border: 1px solid #ddd;
  border-radius: 3px;
  padding: 0 5px;
  margin-left: 3px;
  text-decoration: none;
  white-space: nowrap;
}
.archive-tags a:hover { color: #333; border-color: #aaa; }
</style>

{% assign posts_by_year = site.posts | group_by_exp: "post", "post.date | date: '%Y'" %}
{% for year in posts_by_year %}
### {{ year.name }}

<ul class="archive-list">
{% for post in year.items %}
<li>
  <span class="archive-date">{{ post.date | date: "%m-%d" }}</span>
  <span class="archive-title"><a href="{{ post.url | relative_url }}">{{ post.title }}</a></span>
  {% if post.tags.size > 0 %}
  <span class="archive-tags">{% for tag in post.tags %}<a href="{{ '/tags/' | relative_url }}#{{ tag | slugify }}">{{ tag }}</a>{% endfor %}</span>
  {% endif %}
</li>
{% endfor %}
</ul>
{% endfor %}
