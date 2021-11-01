---
layout: default
title: Collection of Tutorials
---

{% for t in site.tutorials %}

<h2><a href="{{ t.url"" | prepend: site.baseurl | prepend: site.url }}">{{ t.title }}</a></h2>

<p class="post-excerpt">{{ t.description | truncate: 160 }}</p>

{% endfor %}  