---
layout: default
title: 50.002 Lecture Notes
---

{% for t in site.lecturenotes %}

<h2><a href="{{ t.url"" | prepend: site.baseurl | prepend: site.url }}">{{ t.title }}</a></h2>

<p class="post-excerpt">{{ t.description | truncate: 160 }}</p>

{% endfor %}  