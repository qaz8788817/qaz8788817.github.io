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
而```mcflirt```會選定一個基準時間點，  
將幾百張的 3D 影像進行平移與旋轉，確保整部4D影片從頭到尾的解剖位置完美對齊。  
同時，這一步也會生成一張清晰的「3D 平均大腦圖」，作為後續步驟的基底。
```
mcflirt -in ${folder}/mreg_trimmed.nii.gz -out ${folder}/mreg_mcf -plots -meanvol
```
參數```-plots```會輸出位移參數的文字檔(可以用來檢查晃動嚴不嚴重)  
```-meanvol```則是把校正後的所有時間點疊加起來，算出一張清晰的「3D 平均大腦圖」。

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
```bet```加上```-m```參數，會偵測大腦邊界，並剪裁出一個黑白的「3D遮罩 (Mask)」(大腦內部數值是 1，外部是 0)。    
```fslmaths -mas```就像是「套用圖層遮罩」。它把原本的4D影片乘上這把3D剪刀，大腦外面的訊號乘上0瞬間變黑，大腦內部的4D波動則完美保留。  

### 4. 空間平滑化(Spatial Smoothing)
「為影像加上一點柔焦，讓不同人的大腦更容易產生共鳴。」  
每個人的大腦溝回就像指紋一樣獨一無二。  
如果我們要把A患者與B患者的腦區拿來做統計比較，太過銳利的解剖邊界反而會造成誤差。  
利用```fslmaths -s```加上適度的高斯模糊，  
不僅能讓不同受試者的腦區在對齊時更容易重疊，還能互相抵銷掉相鄰像素間的隨機雜訊，大幅提升SNR。  
```
fslmaths ${folder}/mreg_brain.nii.gz -s 2.54 ${folder}/mreg_smoothed
```
```-s 2.54```是高斯模糊的標準差(Sigma)。這對應到大約6mm的FWHM(半高全寬)，是VLF分析中非常標準的平滑程度。  

### 5. 高通濾波(High-pass Filtering)
「濾除機器的物理雜訊，萃取純粹的神經波動。」  
MRI掃描儀運作久了，機器發熱會導致影像產生一種週期極長的「緩慢漂移(Scanner Drift)」。  
這純粹是物理雜訊，與大腦活動無關。  
我們設定125秒(0.008 Hz)的截止點，把這些像海浪退潮般的超低頻雜訊切除。  
(注意：FSL 的高通濾波會連同大腦的背景亮度一起歸零，讓大腦在畫面上變成隱形的黑洞，因此濾波後必須執行 ```-add``` 將大腦平均亮度加回來，訊號才算完整！)  
```
fslmaths ${folder}/mreg_smoothed.nii.gz -bptf 78.125 -1 ${folder}/mreg_hpf
```
參數```-bptf```是濾波器。```78.125```是精準算出的高通截止點(對應0.008 Hz或 125秒)。後面的```-1```代表「不進行低通濾波」，因為要保留所有較快的正常大腦脈動。  

```
fslmaths ${folder}/mreg_smoothed.nii.gz -Tmean ${folder}/mreg_mean
fslmaths ${folder}/mreg_hpf.nii.gz -add ${folder}/mreg_mean ${folder}/mreg_hpf_restored
```

### 6. 標準空間對齊(Registration to MNI space)
「將每個獨特的大腦，搬進全世界通用的標準地圖裡。」  
為了向全世界的神經科學家說明發現了「哪個腦區」有異常，我們必須把患者原生的大腦，變形拉伸到全球通用的MNI標準模板上。  
- 第一行```flirt```負責「算數學」：比對患者大腦與標準大腦的差異，算出一張包含平移、縮放、旋轉參數的「變形矩陣地圖」。
- 第二行```applywarp```負責「搬家」：拿著這份地圖，精準地將4D影片裡的每一個時間點，無損地搬遷到標準空間中。
```
flirt -in ${folder}/mreg_hpf_restored.nii.gz -ref MNI152_T1_4mm_brain.nii.gz -out ${folder}/mreg_mean_std -omat ${folder}/reg.mat -bins 256 -cost corratio -searchrx -90 90 -searchry -90 90 -searchrz -90 90 -dof 12
applywarp -i ${folder}/mreg_hpf.nii.gz -r MNI152_T1_4mm_brain.nii.gz --premat=${folder}/reg.mat -o ${folder}/mreg_std.nii.gz
```

### 結語
現在就可以用```fsleyes```去觀察EPI影像的訊號了。  
<div style="background-color: #f9f9f9; padding: 20px; border-radius: 10px; margin: 20px 0; text-align: center;">
  <img src="{{ '/assets/images/MREG//MREG_1.png' | relative_url }}" 
       alt="對齊到MNI空間前後的訊號波形圖差異" 
       style="max-width: 100%; height: auto; border: 1px solid #ddd;">
  <p style="font-size: 0.9em; color: #666; margin-top: 10px;">
    <i>圖：對齊到MNI空間前後的訊號波形圖差異。</i>
  </p>
</div>

要特別注意的是，生成完每一步驟的影像後，用```fslinfo```確認一下影像的attribute ```dim4```是不是大於1。  
指令：```fslinfo file.nii.gz | grep dim4```  

那這篇就先到這裡做個小結！下一篇在繼續做後續的分析~
