---
layout: page
title: Archive
permalink: /archive/
---

{% assign posts_by_year = site.posts | group_by_exp: "post", "post.date | date: '%Y'" %}
{% for year in posts_by_year %}
### {{ year.name }}

{% for post in year.items %}
- [{{ post.title }}]({{ post.url | relative_url }}) <span style="color:#aaa; font-size:0.9em;">{{ post.date | date: "%m-%d" }}</span>{% if post.tags.size > 0 %} {% for tag in post.tags %}`{{ tag }}`{% endfor %}{% endif %}
{% endfor %}
{% endfor %}
