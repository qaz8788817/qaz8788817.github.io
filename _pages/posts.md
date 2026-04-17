---
layout: archive
title: "Blog Posts"
permalink: /posts/
author_profile: true
---

<ul class="taxonomy__index">
  <li>
    <a href="#all-posts"><strong>All Posts</strong></a>
  </li>
  {% for category in site.categories %}
    <li>
      <a href="#{{ category[0] | slugify }}">
        <strong>{{ category[0] }}</strong> <span class="taxonomy__count">{{ category[1].size }}</span>
      </a>
    </li>
  {% endfor %}
</ul>

<section id="all-posts" class="taxonomy__section">
  <h2 class="archive__subtitle">All Posts</h2>
  <div class="entries-list">
    {% for post in site.posts %}
      {% include archive-single.html type="grid" %}
    {% endfor %}
  </div>
</section>

<hr>

{% for category in site.categories %}
  <section id="{{ category[0] | slugify }}" class="taxonomy__section">
    <h2 class="archive__subtitle">Category: {{ category[0] }}</h2>
    <div class="entries-list">
      {% for post in category[1] %}
        {% include archive-single.html type="grid" %}
      {% endfor %}
    </div>
    <a href="#page-title" class="back-to-top">Back to Top &uarr;</a>
  </section>
{% endfor %}