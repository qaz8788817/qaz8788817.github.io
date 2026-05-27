---
layout: single
title: "AI-Driven Home Fall Hazard Detection System"
excerpt: "An AI-powered system leveraging the YOLO architecture to detect home fall risks via mobile photography. It integrates questionnaire-based data to produce standardized, objective home environment assessment reports."
header:
  teaser: /assets/images/default_teaser.png
author_profile: true
# sidebar:
#   nav: "main"
---

2024年，與職能治療學系的老師共同完成此系統。

### 動機
居家訪視成為職能治療師迫在眉睫的問題，如何讓獨居長者或行動不便者在家中也能維持良好的生活機能，避免有任何容易使人受傷的擺設或位置，職能治療師往往需要花費人力、時間、金錢在於往返個案家中。一次訪視無法確保一切都已安排妥當，兩次、三次的來回奔波，職能治療師需要有更節省時間且人力的輔助工具。

### 方法
透過本系統，個案可以在家就能使用手機拍攝家中環境，系統會將影像自動辨識出可能會造成危險的擺設，並自動化生成問卷供使用者填答，使用者依照實際情況進行答覆後，此問卷就會自動傳到職能治療師的檔案中，輔助治療師能迅速了解個案的狀況並視情況提供幫助與合適的建議。  

本系統使用YoLo v5的模型架構，完成訓練後，將權重檔放入手機網頁中，藉由html與Python完成交互式的網頁。

並利用html2pdf的方式，將問卷、影像與答覆製成一結果表，自動傳給職能治療師。

