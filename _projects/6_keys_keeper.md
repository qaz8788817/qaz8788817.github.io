---
layout: single
title: "A Tool Can Save Ur Keys"
excerpt: "Using Python to generate a easy tool can save your keys in your desktop."
header:
  teaser: /assets/images/keys-keeper-1.png
gallery:
  - url: /assets/images/keys-keeper-1.png
    image_path: /assets/images/keys-keeper-1.png
    alt: "App實際介面"
  - url: /assets/images/keys-keeper-2.png
    image_path: /assets/images/keys-keeper-2.png
    alt: "App實際介面-2"
author_profile: true
---
完成時間：2026/05/25

### 動機
現在網路上申請會員都會希望使用者設定一個很高難度的密碼，  
Google Chrome有自動儲存密碼的功能，  
可是我總會不放心，  
之前有一次我的帳號就被別人登入，  
是還好我有設定第二層驗證，才沒有被其他人登入。  

但是這麼多形式的密碼我也記不起來，  
所以我就vibe-coding了一個可以記錄我的密碼的tool，  
這樣我下次就可以直接複製我的密碼，不用試錯5、6、7、8次了。  

### 功能介紹
以下是功能介紹，就是一個很簡單的小工具：  
* 主密碼防禦解密：啟動時必須輸入唯一的主密碼，透過十萬次 PBKDF2 演算法衍生金鑰，密碼錯誤便完全無法讀取檔案。  
* 本地端硬碟強加密：所有儲存的資料皆不經由第三方伺服器，直接在本地端以業界標準的 Fernet 對稱式演算法進行軍規級加密。  
* 分頁與卡片式管理：介面劃分為「API 金鑰」與「網站帳密」兩大分頁，並以精緻的卡片排版分門別類管理你的憑證。  
* 動態隱私文字遮罩：畫面上敏感的 Key 或密碼預設皆會顯示為 ••••••••，點擊旁邊的眼睛圖標才會短暫切換成明文防止偷看。  
* 剪貼簿一鍵快速複製：卡片右側內建一鍵複製按鈕，點擊便能直接將複雜的金鑰寫入系統剪貼簿，省去手動圈選文字的時間。  
* 即時防呆刪除與連動：提供卡片獨立的垃圾桶按鈕與防誤觸刪除彈窗，確認移除後檔案會在背景自動重新加密存盤。  

大概就是這樣啦~以上！

### App介面
我之後會努力學習，把介面變得能看一點！
{% include gallery %}