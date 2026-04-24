---
layout: single
title: "dMRI技術專欄 (1)：從維度破解大腦纖維的「偽裝」—— Fixel-based Analysis"
# collection: research
topic: FBA  # 這裡自定義妳的分類
excerpt: "探討 FDC 指標如何結合微觀與宏觀資訊，解決 DTI 在複雜纖維交叉處的侷限性。"
header:
  teaser: /assets/images/default_teaser.png
author_profile: true
# sidebar:
#   nav: "main"
---
### 引言
在擴散磁振造影（difussion MRI, dMRI）的世界裡，學者一直在追求更精確地描繪大腦的神經結構。從最早的 DTI 到現在的 FBA、NODDI，這不僅是模型的演進，更是一場關於「空間解析度」與「生物物理意義」的革命。  

### 1. 傳統 DTI 的困境：被「平均」掉的真相
傳統的 Diffusion Tensor Imaging (DTI) 假設每個像素（Voxel）內只有一個主方向（單一橢圓球）。  
* 交叉纖維 (Crossing Fibers)：然而，大腦中約有 60-90% 的區域包含多個方向的神經纖維。當兩條纖維在同一個像素內交叉時，DTI 會將兩者混淆，算出一個圓形的張量。  
* 指標失效：這會導致常用的指標（如 FA 值）在纖維交叉處出現「偽性下降」。我們無法判斷這是因為神經真的受損，還是模型單純無法解析複雜的交通網。  

### 2. 認識新單位：從 Voxel 到 Fixel
![圖片描述]({{ '/assets/images/FBA/FBA(1)/fod2fixel.png' | relative_url }})
*圖 1：voxel與Fixel*  
為了突破傳統模型的限制，Fixel-based Analysis (FBA) 引入了全新的分析單位。  
* Voxel（體素）：是影像中的一個 3D 小方塊，它是空間位址，但沒有方向性。  
* Fixel（纖維元素）：是在「特定像素」內的「特定方向」纖維成分。  
透過高等級的擴散模型（如 CSD, Constrained Spherical Deconvolution），我們可以在同一個像素裡拆解出多個 Fixel。這意味著我們終於能針對「某一條特定路徑」進行獨立測量，而不再受鄰近交叉路徑的干擾。


### 3. FBA 的靈魂：三大核心指標
FBA 最強大的地方在於它提供了三個具備明確生物物理意義的指標。我們可以使用 LaTeX 公式來精確定義它們：  

#### A. Fibre Density (FD)
$ FD $ 衡量的是在單一 Fixel 內的軸突（Axon）數量。  
* 意義：反映了神經纖維內部的微觀完整性。如果 $ FD $ 下降，通常代表軸突本身的流失。  

#### B. Fibre Cross-section (FC)
$ FC $ 衡量的是纖維束整體在空間中所佔據的截面積大小。  
* 意義：這是一種形態學上的測量。反映了神經束整體的粗細變化，常用於偵測神經萎縮（Atrophy）或發育導致的束體擴張。  

#### C. Fibre Density and Cross-section (FDC)
這是 FBA 架構中最具代表性的指標：  
$$FDC = FD \times FC_{norm}$$
FDC 代表了該路徑總體的「傳導效能」。 它結合了微觀的「纖維多不多」與宏觀的「路徑寬不寬」。只有當這兩個維度同時被考慮時，我們才能對大腦白質的結構完整性給出最全面的評價。  

### 4. 結語：為什麼研究者應該轉向 FBA？
FBA 的出現，讓我們從「看一團纖維」進化到「看每一條纖維」。  
這種**「方向特異性 (Directional Specificity)」**讓我們能更敏感地偵測到細微的白質異質性變化，這在精準醫療與基礎神經科學研究中，都是不可或缺的利器。

