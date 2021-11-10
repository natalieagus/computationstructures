---
layout: default
title: 50.002 Problem Sets
---

{% for t in site.problemsets %}

<h2><a href="{{ t.url | prepend: site.baseurl | prepend: site.url }}">{{ t.title }}</a></h2>

<p class="post-excerpt">{{ t.description | truncate: 160 }}</p>

{% endfor %}  