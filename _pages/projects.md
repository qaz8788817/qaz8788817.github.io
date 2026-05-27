---
layout: archive
title: "Projects"
permalink: /projects/
author_profile: true
---
大部分的專案都可以在我的Github上找到哦！  

<style>
  /* 控制按鈕與分頁容器樣式 */
  .toolbar-container {
    display: flex;
    justify-content: space-between;
    align-items: center;
    flex-wrap: wrap;
    gap: 15px;
    margin-bottom: 20px;
  }
  .sort-controls {
    display: flex;
    gap: 10px;
  }
  .sort-btn {
    padding: 8px 16px;
    border: 1px solid #ddd;
    background-color: #fff;
    border-radius: 20px;
    cursor: pointer;
    font-size: 0.9em;
    transition: all 0.2s ease;
  }
  .sort-btn.active {
    background-color: #333;
    color: #fff;
    border-color: #333;
  }

  /* 下方分頁控制鈕 */
  .pagination-controls {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 15px;
    margin-top: 30px;
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

  /* 專案卡片網格 */
  .project-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    gap: 20px;
    margin-top: 20px;
  }
  .project-card {
    border: 1px solid #eee;
    border-radius: 12px;
    overflow: hidden;
    transition: all 0.3s ease;
    text-decoration: none !important;
    color: inherit;
    display: flex;
    flex-direction: column;
  }
  .project-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 10px 20px rgba(0,0,0,0.1);
  }
  .project-img {
    width: 100%;
    height: 180px;
    object-fit: cover;
    flex-shrink: 0;
  }
  .project-info {
    padding: 15px;
    flex-grow: 1;
    display: flex;
    flex-direction: column;
  }
  .project-title {
    margin: 0 0 10px 0 !important;
    font-size: 1.25em !important;
    font-weight: bold;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
    min-height: 2.6em;
    line-height: 1.3;
  }
  .project-excerpt {
    font-size: 0.9em;
    color: #666;
    line-height: 1.5;
    margin: 0;
    display: -webkit-box;
    -webkit-line-clamp: 3;
    -webkit-box-orient: vertical;
    overflow: hidden;
    height: 4.5em;
  }
</style>

<div class="toolbar-container">
  <div class="sort-controls">
    <button class="sort-btn active" id="sort-newest" onclick="changeOrder('desc')">最新 $\rightarrow$ 最舊</button>
    <button class="sort-btn" id="sort-oldest" onclick="changeOrder('asc')">最舊 $\rightarrow$ 最新</button>
  </div>
</div>

<div class="project-grid" id="project-container">
  {% assign sorted_projects = site.projects | sort: "path" %}
  {% for project in sorted_projects %}
    <a href="{{ project.url | relative_url }}" class="project-card" data-path="{{ project.path }}">
      <img src="{{ project.header.teaser | relative_url }}" class="project-img" loading="lazy">
      <div class="project-info">
        <h3 class="project-title">{{ project.title }}</h3>
        <p class="project-excerpt">{{ project.excerpt | strip_html }}</p>
      </div>
    </a>
  {% endfor %}
</div>

<div class="pagination-controls" id="pagination-container"></div>

<script>
  // 全域變數配置
  const itemsPerPage = 10;   // 妳設定的「10個換一頁」
  let currentDirection = 'desc'; 
  let currentPage = 1;
  let allCards = [];

  // 初始化：當頁面載入完成時抓取所有卡片節點
  document.addEventListener("DOMContentLoaded", function() {
    const container = document.getElementById('project-container');
    // 將原始卡片存入陣列，後續直接對陣列操作排序與切片
    allCards = Array.from(container.getElementsByClassName('project-card'));
    
    // 執行初始化渲染（預設倒序、第一頁）
    updateGallery();
  });

  // 當使用者點擊排序按鈕時觸發
  function changeOrder(direction) {
    currentDirection = direction;
    currentPage = 1; // 切換排序時，自動跳回第一頁
    updateGallery();
  }

  // 跳頁核心功能
  window.goToPage = function(targetPage) {
    currentPage = targetPage;
    updateGallery();
    // 自動捲動回列表頂部，優化手機端體驗
    document.getElementById('sort-newest').scrollIntoView({ behavior: 'smooth' });
  };

  // 核心控制鏈：整合【排序】與【分頁】
  function updateGallery() {
    const container = document.getElementById('project-container');
    const paginator = document.getElementById('pagination-container');

    // 1. 先對所有卡片進行排序
    allCards.sort((a, b) => {
      const pathA = a.getAttribute('data-path').toLowerCase();
      const pathB = b.getAttribute('data-path').toLowerCase();
      
      if (currentDirection === 'asc') {
        return pathA.localeCompare(pathB, undefined, {numeric: true, sensitivity: 'base'});
      } else {
        return pathB.localeCompare(pathA, undefined, {numeric: true, sensitivity: 'base'});
      }
    });

    // 2. 計算分頁範圍
    const totalPages = Math.ceil(allCards.length / itemsPerPage);
    const start = (currentPage - 1) * itemsPerPage;
    const end = start + itemsPerPage;

    // 3. 清空原容器，並只將當前頁面的 10 張卡片塞入 DOM
    container.innerHTML = '';
    allCards.forEach((card, index) => {
      if (index >= start && index < end) {
        container.appendChild(card);
      }
    });

    // 4. 動態渲染底部的分頁按鈕
    if (totalPages <= 1) {
      paginator.innerHTML = ''; // 如果小於或等於 10 個專案，就不顯示分頁按鈕
    } else {
      paginator.innerHTML = `
        <button class="page-btn" ${currentPage === 1 ? 'disabled' : ''} onclick="window.goToPage(${currentPage - 1})">上一頁</button>
        <span class="page-info">${currentPage} / ${totalPages}</span>
        <button class="page-btn" ${currentPage === totalPages ? 'disabled' : ''} onclick="window.goToPage(${currentPage + 1})">下一頁</button>
      `;
    }

    // 5. 切換頂部排序按鈕的 active 樣式
    document.getElementById('sort-newest').classList.toggle('active', currentDirection === 'desc');
    document.getElementById('sort-oldest').classList.toggle('active', currentDirection === 'asc');
  }
</script>