---
layout: single
title: "Research Library"
permalink: /research/
author_profile: true
entries_layout: grid  # 如果妳想要卡片式排版，可以用 grid；想要列表式可以用 list
---

這裡是我的研究知識庫，包含 dMRI 指標、機器學習與臨床應用。

{% assign research_groups = site.research | group_by: 'topic' %}

{% for group in research_groups %}
  <h2 id="{{ group.name | slugify }}" class="archive__subtitle">{{ group.name }}</h2>
  <div class="entries-{{ page.entries_layout | default: 'list' }}">
    {% for post in group.items %}
      {% include archive-single.html %}
    {% endfor %}
  </div>
{% endfor %}