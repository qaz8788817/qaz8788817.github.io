---
layout: single
title: "ghost removal online web"
excerpt: "Setting up a ghost removal web for MRI data, and making researchers use the tool more easier and faster."
header:
  teaser: /assets/images/default_teaser.png
author_profile: true
# sidebar:
#   nav: "main"
---
2023/07 - 2023/08於臺灣國家衛生研究院實習期間所完成的線上系統。

### 簡介
dMRI腦影像在掃描完之後，需要經過多項前處理才能進行後續的資料分析，然而這些處理都需要透過程式碼指令才能運作。
複雜的指令對一般的學者較為困難，包含參數設定、檔案路徑、指令等，有些需要經過特定的順序與邏輯才能完成。
為了讓這件事情變得普及化，且能夠有整齊、平行化的計算與呈現，利用Pyhton Flask與Bash Script、html整合資料庫(MySQL)，完成了這個線上版的Ghost Removal。