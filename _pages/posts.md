---
layout: archive
title: "Blog Posts"
permalink: /posts/
author_profile: true
---

<style>
  /* 1. 統一列表樣式，解決重疊問題 */
  .list__item {
    display: block;
    margin-bottom: 30px;
    clear: both;
    overflow: hidden;
  }

  /* 2. 強制圖片規格 */
  .archive__item-teaser {
    width: 250px !important; /* 固定寬度 */
    height: 150px !important; /* 固定高度 */
    object-fit: cover;
    float: left; /* 圖片靠左 */
    margin-right: 20px;
    border-radius: 8px;
  }

  /* 3. 確保標題與內文在圖片右側，不重疊 */
  .archive__item-body {
    overflow: hidden;
  }

  .archive__item-title {
    margin-top: 0 !important;
    margin-bottom: 5px !important;
    line-height: 1.3;
  }

  /* 4. 摘要長度限制 (2行) */
  .archive__item-excerpt {
    font-size: 0.9em;
    line-height: 1.5;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }

  /* 5. 分類收納設計 */
  details {
    margin-top: 20px;
    border: 1px solid #eee;
    border-radius: 8px;
  }
  summary {
    padding: 15px;
    cursor: pointer;
    font-weight: bold;
    background: #fafafa;
    list-style: none;
  }
  summary:hover { background: #f0f0f0; }
  .details-content { padding: 20px; }
</style>

<div class="entries-list">
  {% for post in site.posts %}
    <div class="list__item">
      {% include archive-single.html type="list" %}
    </div>
  {% endfor %}
</div>

<hr style="margin: 50px 0;">

<h3 class="archive__subtitle">View by Category</h3>
{% for category in site.categories %}
  <details>
    <summary>{{ category[0] }} ({{ category[1].size }})</summary>
    <div class="details-content">
      {% for post in category[1] %}
        <div class="list__item">
          {% include archive-single.html type="list" %}
        </div>
      {% endfor %}
    </div>
  </details>
{% endfor %}