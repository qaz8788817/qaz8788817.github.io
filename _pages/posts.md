---
layout: archive
title: "Blog Posts"
permalink: /posts/
author_profile: true
---

<style>
  /* 1. 每一列的容器：確保絕對不重疊 */
  .custom-post-item {
    display: flex; /* 圖片在左，文字在右 */
    align-items: flex-start;
    margin-bottom: 40px;
    padding-bottom: 20px;
    border-bottom: 1px solid #eee;
    text-decoration: none !important;
    color: inherit;
    transition: transform 0.2s ease;
  }
  .custom-post-item:hover {
    transform: translateX(10px); /* 輕微右移互動感 */
    background: rgba(0,0,0,0.01);
  }

  /* 2. 圖片規格：強制統一大小，不變形 */
  .post-image-wrapper {
    flex-shrink: 0;
    width: 200px;
    height: 130px;
    margin-right: 25px;
    overflow: hidden;
    border-radius: 6px;
  }
  .post-image-wrapper img {
    width: 100%;
    height: 100%;
    object-fit: cover; /* 關鍵：強制填滿且不拉伸 */
  }

  /* 3. 文字內容區 */
  .post-content-wrapper {
    flex-grow: 1;
  }

  /* 標題：保證完整，不限制長度 */
  .post-item-title {
    margin: 0 0 8px 0 !important;
    font-size: 1.4em !important;
    font-weight: bold;
    color: #333;
    line-height: 1.3;
  }

  /* 摘要：強制截斷為 2 行 */
  .post-item-excerpt {
    font-size: 0.95em;
    color: #666;
    line-height: 1.6;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }

  /* 4. 分類互動匣 (預設隱藏內容) */
  .category-box {
    margin-top: 20px;
    border: 1px solid #ddd;
    border-radius: 8px;
    overflow: hidden;
  }
  .category-box summary {
    padding: 15px;
    background: #f8f9fa;
    cursor: pointer;
    font-weight: bold;
    outline: none;
  }
  .category-box summary:hover {
    background: #e9ecef;
  }
  .category-content {
    padding: 20px;
    background: #fff;
  }
</style>

<h3 class="archive__subtitle">View by Category</h3>
{% for category in site.categories %}
  <details class="category-box">
    <summary>{{ category[0] | capitalize }} ({{ category[1].size }})</summary>
    <div class="category-content">
      {% for post in category[1] %}
        <a href="{{ post.url | relative_url }}" class="custom-post-item">
          <div class="post-image-wrapper">
            <img src="{{ post.header.teaser | relative_url }}" alt="{{ post.title }}">
          </div>
          <div class="post-content-wrapper">
            <h2 class="post-item-title">{{ post.title }}</h2>
            <div class="post-item-excerpt">
              {{ post.excerpt | strip_html | truncatewords: 30 }}
            </div>
          </div>
        </a>
      {% endfor %}
    </div>
  </details>
{% endfor %}