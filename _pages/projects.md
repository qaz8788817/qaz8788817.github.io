---
layout: home
title: "Projects"
permalink: /projects/
author_profile: true
---

<style>
  /* 控制按鈕樣式 */
  .sort-controls {
    margin-bottom: 20px;
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

<div class="sort-controls">
  <button class="sort-btn active" id="sort-newest" onclick="sortProjects('desc')">最新 $\rightarrow$ 最舊</button>
  <button class="sort-btn" id="sort-oldest" onclick="sortProjects('asc')">最舊 $\rightarrow$ 最新</button>
</div>

<div class="project-grid" id="project-container">
  {% assign sorted_projects = site.projects | sort: "path" %}
  {% for project in sorted_projects %}
    <a href="{{ project.url | relative_url }}" class="project-card" data-path="{{ project.path }}">
      <img src="{{ project.header.teaser | relative_url }}" class="project-img">
      <div class="project-info">
        <h3 class="project-title">{{ project.title }}</h3>
        <p class="project-excerpt">{{ project.excerpt | strip_html }}</p>
      </div>
    </a>
  {% endfor %}
</div>

<script>
  function sortProjects(direction) {
    const container = document.getElementById('project-container');
    const cards = Array.from(container.getElementsByClassName('project-card'));
    
    // 根據 data-path 屬性進行排序
    cards.sort((a, b) => {
      const pathA = a.getAttribute('data-path').toLowerCase();
      const pathB = b.getAttribute('data-path').toLowerCase();
      
      // 使用 localeCompare 進行字串自然比對
      if (direction === 'asc') {
        return pathA.localeCompare(pathB, undefined, {numeric: true, sensitivity: 'base'});
      } else {
        return pathB.localeCompare(pathA, undefined, {numeric: true, sensitivity: 'base'});
      }
    });
    
    // 清空容器並重新依序加入卡片
    container.innerHTML = '';
    cards.forEach(card => container.appendChild(card));
    
    // 切換按鈕的 active 樣式
    document.getElementById('sort-newest').classList.toggle('active', direction === 'desc');
    document.getElementById('sort-oldest').classList.toggle('active', direction === 'asc');
  }

  // 頁面載入時預設執行一次最新到最舊（desc）
  document.addEventListener("DOMContentLoaded", function() {
    sortProjects('desc');
  });
</script>