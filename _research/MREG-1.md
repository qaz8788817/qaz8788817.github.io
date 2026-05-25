---
layout: single
title: "超越 BOLD 的速度——解密超快速磁共振腦大腦造影(MREG)"
# collection: research
topic: MREG
excerpt: "探索時間解析度小於100毫秒的造影技術，如何捕捉大腦內部的生理震盪與腦脊髓液流動。"
header:
  teaser: /assets/images/default_teaser.png
author_profile: true
toc: true
toc_label: "技術索引"
toc_icon: "bolt"
toc_sticky: true
---

### 引言
我們探討過如何使用FBA觀察大腦的「硬體結構」。  
然而，大腦不僅是靜態的線路，更是動態的電信號與生理流體中心。  
傳統的fMRI受到時間解析度的限制，難以捕捉快速的生理變化。  
這時，**MREG (Magnetic Resonance Encephalography)** 應運而生，成為我們觀測大腦動態生理活動的「高速顯微鏡」。  

### 1. 速度的革命：為什麼我們需要 MREG？
傳統的BOLD fMRI通常需要2秒左右才能完成一次全腦掃描(TR = 2s)。  
這意味著我們只能觀察到低頻的訊號波動(< 0.1 Hz)。  

* **MREG 的突破**：透過特殊的神經造影序列，MREG 能將 TR 縮短至 **100 毫秒 (0.1s)** 甚至更低。  
* **物理意義**：根據奈奎斯特採樣定理（Nyquist Theorem），更高的採樣率讓我們能捕捉到更高頻的訊號： 

  $$f_{max} = \frac{1}{2 \times TR}$$  
  
  當 $TR = 0.1s$ 時，$f_{max}$ 可達 $5 Hz$，這足以讓我們完整觀測到心跳與呼吸引起的腦部訊號震盪。  

### 2. 從「噪訊」變「訊號」：生理訊號的價值
在傳統 fMRI 研究中，心跳與呼吸通常被視為需要剔除的「噪訊」。  
但在 MREG 的視角下，這些訊號是研究大腦健康的關鍵。  

<div style="background-color: #f0f7ff; padding: 20px; border-radius: 10px; margin: 20px 0;">
  <h4>MREG 的核心優勢</h4>
  <ul>
    <li><strong>生理震盪捕捉</strong>：精確區分心臟跳動(~1.0 Hz)與呼吸(~0.3 Hz)對腦壓的影響。</li>
    <li><strong>類淋巴系統(Glymphatic System)</strong>：觀測腦脊髓液(CSF)如何隨脈動清除大腦代謝廢物。</li>
    <li><strong>即時功能連接</strong>：觀察腦區之間更即時、非穩態的訊號傳遞。</li>
  </ul>
</div>



### 3. 技術挑戰：訊號處理的深水區
由於MREG的採樣速度極快，數據量也呈現爆炸性增長，且訊號中夾雜了複雜的生理偽影。
* **訊號去噪(De-noising)**：如何在超高頻的數據中，準確過濾儀器雜訊並保留微弱的生理震盪。  
* **頻域分析**：利用快速傅立葉變換(FFT)或小波轉換(Wavelet Transform)觀察特定頻段(如 $0.027 - 0.073 Hz$)的訊號強度。  

### 4. 臨床應用前景
MREG的高速特性使其在神經退化性疾病(如阿茲海默症)與癲癇研究中極具潛力。  
當大腦的「清理機制」(類淋巴系統)出現障礙時，MREG 捕捉到的脈動訊號往往會出現異常。  

### 結語
從dMRI的微觀結構分析，到MREG的超快速動態監測，影像技術的演進正不斷打破我們對大腦的認知邊界。  