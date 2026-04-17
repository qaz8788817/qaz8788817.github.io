---
layout: archive
title: "Blog Posts"
permalink: /posts/
author_profile: true
---
<div class="entries-list">
  {% for post in site.posts %}
    {% include archive-single.html type="list" %}
  {% endfor %}
</div>