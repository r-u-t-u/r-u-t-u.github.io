---
title: Articles
layout: defaults/page
permalink: index.html
narrow: true
---


{% for post in site.posts %}
<h2><a class="post-link" href="{{site.baseurl}}{{post.url}}">
    <u><b>{{ post.title }}</b></u>
</a></h2>
<!-- {{ post.date | date: '%d %B %Y' }} -->
{% endfor %}


