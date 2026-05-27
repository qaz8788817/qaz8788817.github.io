---
layout: archive
title: "Projects"
permalink: /projects/
author_profile: true
---

<style>
  /* 讓專案列表看起來更像專業作品集 */
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
    flex-direction: column; /* 讓內容垂直排列 */
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
    flex-grow: 1; /* 讓資訊區自動填滿剩下的空間 */
    display: flex;
    flex-direction: column;
  }

  /* 1. 統一標題高度 (最多 2 行) */
  .project-title {
    margin: 0 0 10px 0 !important;
    font-size: 1.25em !important;
    font-weight: bold;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
    min-height: 2.6em; /* 即使只有一行，也會佔據兩行的空間，確保對齊 */
    line-height: 1.3;
  }

  /* 2. 統一摘要高度 (最多 3 行) */
  .project-excerpt {
    font-size: 0.9em;
    color: #666;
    line-height: 1.5;
    margin: 0;
    display: -webkit-box;
    -webkit-line-clamp: 3; /* 限制顯示三行 */
    -webkit-box-orient: vertical;
    overflow: hidden;
    height: 4.5em; /* 固定三行的高度 (1.5 * 3) */
  }
</style>

<div class="project-grid">
  {% comment %} 1. 先依照 path（即包含檔案名稱的完整路徑）進行排序 {% endcomment %}
  {% assign sorted_projects = site.projects | sort: "path" %}
  
  {% comment %} 2. 再將其反轉，達成由大到小（由新到舊或倒序）排序 {% endcomment %}
  {% assign reversed_projects = sorted_projects | reverse %}

  {% for project in reversed_projects %}
    <a href="{{ project.url | relative_url }}" class="project-card">
      <img src="{{ project.header.teaser | relative_url }}" class="project-img">
      <div class="project-info">
        <h3 class="project-title">{{ project.title }}</h3>
        <p class="project-excerpt">{{ project.excerpt | strip_html }}</p>
      </div>
    </a>
  {% endfor %}
</div>