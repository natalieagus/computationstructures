---
layout: default
title: 50.002 FPGA Resources
---

{% for t in site.fpga %}

<h2><a href="{{ t.url | prepend: site.baseurl | prepend: site.url }}">{{ t.title }}</a></h2>

<p class="post-excerpt">{{ t.description | truncate: 160 }}</p>

{% endfor %}  