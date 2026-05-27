---
layout: single
title: "A Tool Can Edit PDF file"
excerpt: "Using Python to generate a easy tool can edit pdf files more easily in Windows system without internet."
header:
  teaser: /assets/images/pdf_edit.png
gallery:
  - url: /assets/images/pdf_edit.png
    image_path: /assets/images/pdf_edit.png
    alt: "App實際介面"
author_profile: true
---
完成時間：2026/04/27

### 動機
在處理一些文書事項的時候，常常需要把pdf檔案進行拆分、合併、刪除頁面等等，  
然後Windows又不像Mac可以直接對pdf進行編輯，  
所以網路上就出現了很多線上的pdf編輯器，  
但是萬一臨時斷線或是出了什麼trouble又很麻煩，  
所以我決定自己做一個小tool放在桌面，  
需要的時候按兩下就好了！  

### 功能介紹
我以我自己最常使用的幾個功能做介紹：  
* 合併PDF：針對多個PDF檔案進行合併，順序是依照放入List的順序進行合併並輸出成新的檔案。  
* 刪除特定PDF頁面：刪除指定的PDF頁碼，重新匯出新的PDF檔案。  
* 提取特定PDF頁面：從PDF裡面抓取特定頁碼，重新輸出成新的PDF。  
* 提取並拆分PDF頁面：將指定的PDF頁碼提取出來後，分別存成各自的PDF檔案。  
* PDF大小固定A4：將所有PDF檔案縮放成A4大小。  

### App介面
這是最後的介面，等有空我再把它變好看一點點！  
{% include gallery %}