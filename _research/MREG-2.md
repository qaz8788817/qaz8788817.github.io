---
layout: single
title: "超快速磁共振腦大腦造影(MREG)處理步驟 (EPI version)"
# collection: research
topic: MREG
excerpt: "利用EPI影像先簡單實行前處理的步驟，之後在MREG data中就可以直接使用。"
header:
  teaser: /assets/images/MREG/MREG_1.png
author_profile: true
toc: true
toc_label: "技術索引"
toc_icon: "bolt"
toc_sticky: true
---

### 引言
大腦是一座交響樂團，而 fMRI 掃描儀就是我們用來錄音的麥克風。  
當我們按下錄音鍵時，錄到的往往不只有美妙的樂曲(神經活動)，  
還混雜了樂手移動椅子的聲音(頭部晃動)、場外機器的運轉聲(掃描儀發熱漂移)，  
甚至是隔壁房間的噪音(頭骨與肌肉血流)。  

要如何從這片吵雜的原始錄音中，萃取出我們想要的極低頻交響樂呢？  
這正是神經影像預處理的核心任務。  
這篇文章將為你拆解MREG數據分析的關鍵步驟。  
我們將使用FSL (FMRIB Software Library)的標準化影像預處理管線(pipeline)，  
像數據法醫一樣層層過濾雜訊，最終把抽象的4D腦部影片，化為能揭開大腦網路奧秘的統計數據。  

以下開始進行預處理的步驟，但因為尚未拿到真正的MREG資料，只能先以當時掃描的EPI影像(大腦定位檔)進行展示。

### 1. 剔除不穩定訊號(Dummy Scans)
「就像發動汽車需要暖機，MRI 剛開機的訊號是不可靠的。」  
磁振造影開始掃描的前幾秒，磁場尚未達到熱平衡，這時候錄到的腦波訊號會劇烈飄移。  
我們使用fslroi，像剪刀一樣，把最前面 8 秒的不穩定畫面剪掉，  
確保後續進入分析的每一格畫面都是高品質的穩定數據。  
```
fslroi ${folder}/14.nii ${folder}/mreg_trimmed.nii 10 -1
```
參數```10 -1```：代表「從第 10 個時間點開始保留，一直保留到最後（-1 代表結尾）」。對於 TR=0.8 秒的 EPI，這剛好切掉了最不穩定的前 8 秒，確保後續分析的訊號品質。

### 2. 頭部動態校正 (Motion Correction)
「把晃動的受試者，牢牢釘在同一個座標上。」  
在5分鐘的掃描過程中，受試者一定會因為呼吸、吞嚥而產生微小的頭部晃動。  
如果沒有校正，上一秒的「大腦額葉」到了下一秒可能會跑到別的座標去。  
```mcflirt```會選定一個基準時間點，  
將幾百張的 3D 影像進行平移與旋轉，確保整部4D影片從頭到尾的解剖位置完美對齊。  
同時，這一步也會生成一張清晰的「3D 平均大腦圖」，作為後續步驟的基底。
```
mcflirt -in ${folder}/mreg_trimmed.nii.gz -out ${folder}/mreg_mcf -plots -meanvol
```
參數```-plots ```會輸出位移參數的文字檔(可以用來檢查晃動嚴不嚴重)  
```-meanvol ```則是把校正後的所有時間點疊加起來，算出一張清晰的「3D 平均大腦圖」。

### 3. 剝頭皮與去雜訊 (Brain Extraction)
「幫大腦去蕪存菁，只留下我們關心的神經組織。」  
頭骨、肌肉和頭皮的血流雜訊非常強烈，會嚴重干擾數據分析。  
為了不破壞 4D 影片的時間軸，我們採用「兩步法」：  
1. 先利用剛才的 3D 平均影像，精準計算出大腦的輪廓，做出一把黑白的「3D遮罩(Mask)」。  
2. 利用```fslmaths```把這把剪刀套用到原本的4D影片上。遮罩外的地方數值歸零，遮罩內的大腦神經波動則完美保留。  
```
bet ${folder}/mreg_mcf.nii.gz ${folder}/mreg_brain -f 0.25 -g 0.22 -m
fslmaths ${folder}/mreg_mcf.nii.gz -mas ${folder}/mreg_brain_mask.nii.gz ${folder}/mreg_brain
```

### 結語
從dMRI的微觀結構分析，到MREG的超快速動態監測，影像技術的演進正不斷打破我們對大腦的認知邊界。  