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

  /* 5. 前端分頁按鈕樣式 */
  .pagination-controls {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 15px;
    margin-top: 20px;
    padding-top: 10px;
  }
  .page-btn {
    padding: 6px 14px;
    border: 1px solid #ccc;
    background: #fff;
    border-radius: 4px;
    cursor: pointer;
    font-size: 0.85em;
    transition: all 0.2s;
  }
  .page-btn:hover:not(:disabled) {
    background: #f0f0f0;
    border-color: #888;
  }
  .page-btn:disabled {
    opacity: 0.4;
    cursor: not-allowed;
  }
  .page-info {
    font-size: 0.9em;
    color: #555;
  }
</style>

<h3 class="archive__subtitle">View by Category</h3>

{% for category in site.categories %}
  <details class="category-box" id="cat-box-{{ forloop.index }}">
    <summary>{{ category[0] | capitalize }} ({{ category[1].size }})</summary>
    <div class="category-content">
      
      <div class="post-list-container">
        {% for post in category[1] %}
          <a href="{{ post.url | relative_url }}" class="custom-post-item">
            <div class="post-image-wrapper">
              <img src="{{ post.header.teaser | relative_url }}" alt="{{ post.title }}" loading="lazy">
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

      <div class="pagination-controls" id="page-ctrl-{{ forloop.index }}"></div>

    </div>
  </details>
{% endfor %}

<script>
  document.addEventListener("DOMContentLoaded", function() {
    const itemsPerPage = 5; // 妳設定的「5個換一頁」

    // 抓取畫面上所有的分類區塊
    const categoryBoxes = document.querySelectorAll('.category-box');

    categoryBoxes.forEach((box, index) => {
      const container = box.querySelector('.post-list-container');
      const posts = Array.from(container.querySelectorAll('.custom-post-item'));
      const ctrlContainer = box.querySelector('.pagination-controls');
      
      let currentPage = 1;
      const totalPages = Math.ceil(posts.length / itemsPerPage);

      // 如果文章總數小於或等於 5 篇，就不需要顯示分頁按鈕
      if (totalPages <= 1) return;

      // 核心渲染功能：只顯示目前頁數的 5 篇，其餘隱藏
      function renderPage(page) {
        currentPage = page;
        
        const start = (page - 1) * itemsPerPage;
        const end = start + itemsPerPage;

        posts.forEach((post, i) => {
          if (i >= start && i < end) {
            post.style.display = 'flex'; // 顯示符合區間的 Post
          } else {
            post.style.display = 'none'; // 隱藏其他
          }
        });

        // 更新按鈕狀態
        ctrlContainer.innerHTML = `
          <button class="page-btn" ${currentPage === 1 ? 'disabled' : ''} onclick="window.changeCatPage(${index}, ${currentPage - 1})">上一頁</button>
          <span class="page-info">${currentPage} / ${totalPages}</span>
          <button class="page-btn" ${currentPage === totalPages ? 'disabled' : ''} onclick="window.changeCatPage(${index}, ${currentPage + 1})">下一頁</button>
        `;
      }

      // 將控制權限綁定到全域視窗，以便 HTML 按鈕點擊呼叫
      if (!window.catPaginationRegistry) window.catPaginationRegistry = {};
      window.catPaginationRegistry[index] = renderPage;

      // 初始化第一頁
      renderPage(1);
    });

    // 全域跳頁處理函式
    window.changeCatPage = function(catIndex, targetPage) {
      if (window.catPaginationRegistry && window.catPaginationRegistry[catIndex]) {
        window.catPaginationRegistry[catIndex](targetPage);
      }
    };
  });
</script>