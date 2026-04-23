---
layout: single
title: "Research Library"
permalink: /research/
author_profile: true
entries_layout: grid  # 如果妳想要卡片式排版，可以用 grid；想要列表式可以用 list
---

這裡是我的研究知識庫，包含 dMRI 指標、機器學習與臨床應用。

<!-- {% assign research_groups = site.research | group_by: 'topic' %}

{% for group in research_groups %}
  <h2 id="{{ group.name | slugify }}" class="archive__subtitle">{{ group.name }}</h2>
  <div class="entries-{{ page.entries_layout | default: 'list' }}">
    {% for post in group.items %}
      {% include archive-single.html %}
    {% endfor %}
  </div>
{% endfor %} -->

{% for group in research_groups %}
  <h2 id="{{ group.name | slugify }}" class="archive__subtitle">{{ group.name }}</h2>
  
  <div class="list__item">
    {% for post in group.items %}
      <article class="archive__item" itemscope itemtype="https://schema.org/CreativeWork">
        <h3 class="archive__item-title" itemprop="headline">
          <a href="{{ post.url | relative_url }}" rel="permalink">{{ post.title }}</a>
        </h3>
        {% if post.excerpt %}
          <p class="archive__item-excerpt" itemprop="description">
            {{ post.excerpt | markdownify | strip_html | truncate: 160 }}
          </p>
        {% endif %}
      </article>
    {% endfor %}
  </div>
{% endfor %}