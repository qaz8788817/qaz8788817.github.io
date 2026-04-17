---
layout: archive
title: "Blog Posts"
permalink: /posts/
author_profile: true
---

<style>
  /* 限制文章區塊的高度與長度 */
  .archive__item {
    max-height: 400px; /* 限制最高高度 */
    overflow: hidden;  /* 超過部分隱藏 */
    display: flex;
    flex-direction: column;
  }

  /* 限制摘要文字行數 (只顯示 3 行) */
  .archive__item-excerpt {
    display: -webkit-box;
    -webkit-line-clamp: 3;
    -webkit-box-orient: vertical;
    overflow: hidden;
    font-size: 0.9em;
    line-height: 1.5;
    margin-bottom: 10px;
  }
  
  /* 讓分類詳情區塊預設不要太佔空間，或者甚至可以考慮移到獨立頁面 */
</style>

<ul class="taxonomy__index">
  {% for category in site.categories %}
    <li>
      <a href="#{{ category[0] | slugify }}">
        <strong>{{ category[0] }}</strong> <span class="taxonomy__count">{{ category[1].size }}</span>
      </a>
    </li>
  {% endfor %}
</ul>

<div class="entries-list">
  {% for post in site.posts %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>

<hr style="margin-top: 50px;">

<h3 class="archive__subtitle">View by Category</h3>
{% for category in site.categories %}
  <section id="{{ category[0] | slugify }}" class="taxonomy__section">
    <h2 class="archive__subtitle">{{ category[0] }}</h2>
    <div class="entries-list">
      {% for post in category[1] %}
        {% include archive-single.html type="grid" %}
      {% endfor %}
    </div>
    <a href="#page-title" class="back-to-top">Back to Top &uarr;</a>
  </section>
{% endfor %}