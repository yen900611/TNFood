# Day30 露出水面 SEO

editor: 麒麟
tag: SEO, django
進度: 🌕

# 🏁 前言、摘要

<aside>
💡 我們網站完成後，另一個重要的事情就是增加使用者造訪的機會，因此就需要進行SEO（Search Engine Optimization），簡單來説就是增加搜尋引擎對我們網站的友好度，讓搜尋引擎更願意將我們的網站顯示在搜尋結果中。

</aside>

---

# 💡概念說明

## 搜尋引擎如何決定搜尋結果的排行呢？

1. 搜尋引擎會有機器人在網頁上，不斷地去查找各個網站的內容，將文章內容搜尋回來。根據文章的網址、標題、內容擷取關鍵字，若是使用者輸入的關鍵字匹配度愈高，就愈容易將相關聯的頁面顯示在搜尋結果中。
    - 搜尋 `台南不需要米其林`的結果，完全找不到自己的網站
        
        ![search_result.png](Day30%20%E9%9C%B2%E5%87%BA%E6%B0%B4%E9%9D%A2%20SEO%204bc12eaff9704dbf839e66b8245dc96a/search_result.png)
        
2. 除了內容，搜尋引擎機器人會為網站的效能做評分，載入速度愈快，使用者體驗愈好評分愈高。
    - 網站評分工具，可以使用 F12 點選lighthouse即可評分
        
        ![lighthouse.png](Day30%20%E9%9C%B2%E5%87%BA%E6%B0%B4%E9%9D%A2%20SEO%204bc12eaff9704dbf839e66b8245dc96a/lighthouse.png)
        

## 優化SEO的方法

參考****[SEO檢查表](https://domyweb.org/seo-check-list/)****之後可以大約舉出幾個優化方向****。**** 

1. 嵌入關鍵字
    
    網頁的標題、內文、內文中的`h1`~`h5`標籤、網頁中的 `meta` 標籤都是搜尋引擎會檢查的重點。關鍵字愈多，就愈容易切合使用者所下的關鍵字，也就愈容易被看見。
    
    - 前後端分離的缺點
        
        因為搜尋引擎點開網站時，就會去搜尋整個網頁的內文，不會變動的內文稱作靜態網頁，但目前使用前後端分離的狀態，網頁的內文會使用`JS` 來渲染出來，這稱為動態網頁。搜尋引擎會比較傾向不要執行`JS`，因此看到的內文是空空如也。
        
        - 參考 **[SPA 對 SEO 不好？為了 SEO 所以要捨棄 CSR ? 多了解一點原理答案就自然浮現了！**](https://progressbar.tw/posts/297)
2. 改善網站效能
    - 壓縮程式碼的大小
        
        程式碼坦白說就是一堆字串，其中包含很多空白換行等不重要的資訊，去掉空白與換行符，對人類來說可讀性變差，但對瀏覽器是一樣的，反而可以加快傳輸速度。
        
        - 常見的方式是將 `css` `js` 檔案除去空白與換行，也因此有許多`css` `js` 檔名中會包含 `min` ，表示為最小化的版本。
    - 壓縮傳輸資料的大小
        
        另外就是使用壓縮演算法將要傳輸的資料減少大小後，瀏覽器取得之後再解開來。
        
        - 常見的方法是使用gzip 壓縮
    - 壓縮圖片
        
        網站上所使用的圖片，不一定需要很高的解析度，解析度太高反而會影響載入的速度。
        
    - 使用CDN
        - [第七天](https://ithelp.ithome.com.tw/articles/10296250)有提到，使用CDN可以減少伺服器的負擔，因為CDN通常會建立在比較穩定的雲端主機上，速度也會比較快。
3. 增加網站的曝光度
    
    搜尋引擎也會將網站的使用人數、點擊人數作為排名的依據。因此新建立的網站，一開始搜尋不到是正常的，因為搜尋引擎並不知道這個網站的存在。
    
    等到搜尋點擊次數增多，搜尋引擎就會認為這個網站是重要有用的，自然會將該網站的排名往前提升。我們可以使用這個邏輯，讓搜尋引擎知道網站的存在。
    
    - 自己搜尋自己的網站，並且不斷點擊。
        
        這就是要告訴搜尋引擎，自身網站的存在。
        
    - 在不同地方貼上自身網站的連結。
        
        搜尋引擎會在網路上不斷瀏覽網站，如果在別的社群也看到自身的網站網址，這樣也會增加自身網站的曝光度。
        
        > *機器人也會逛網站？這個其實是網路上十分常見的現象，根據統計，百分之六十的網站流量是機器人(bot)造成的。善意的機器人如Googlebot，就負責每天收集數以億計的網站資料，是Google搜尋引擎更新的最重要幫手。既然有善良的機器人，自然就有惡意的機器人。*
        > 
        > 
        > 引用自 [https://www.iware.com.tw/blog-601.html](https://www.iware.com.tw/blog-601.html)
        > 

---

# 🌟本日成果

![Untitled](Day30%20%E9%9C%B2%E5%87%BA%E6%B0%B4%E9%9D%A2%20SEO%204bc12eaff9704dbf839e66b8245dc96a/Untitled.png)

![Untitled](Day30%20%E9%9C%B2%E5%87%BA%E6%B0%B4%E9%9D%A2%20SEO%204bc12eaff9704dbf839e66b8245dc96a/Untitled%201.png)

1. 網頁嵌入關鍵字
    
    參考 [Google 搜尋引擎最佳化 (SEO) 入門指南](https://developers.google.com/search/docs/beginner/seo-starter-guide?hl=zh-tw)
    
    - 增加 `meta` 標籤
        
        ```html
        <!-- food/base.html -->
        <meta name="description" content="台南是美食之都，筆者深受台南美食的囹圄，每到用餐時刻，面對密密麻麻的google Map ，總是不知道該吃什麼比較好。想要快速篩選餐廳，或是推薦給親朋好友，需要花費很多心力，因此我們開發了這個網站，作為自己的美食地圖。Tainan is a city of delicious food. We just don't know how to choose. So we develop this website to record our delicious food map and provide the filter to choose which we should eat this meal.">
        ```
        
    - 增加[結構化資料](https://developers.google.com/search/docs/advanced/structured-data/intro-structured-data?hl=zh-tw)
        
        ```html
        <script type="application/ld+json">
        {
              "@context": "https://tnfood.pythonanywhere.com/food/",
              "@type": "NewsArticle",
              "headline": "台南不需要米其林",
              "image": [
        
               ],
              "datePublished": "2022-10-15T12:00:00+08:00",
              "dateModified": "2022-10-15T20:00:00+08:00",
              "author": [{
                  "@type": "Person",
                  "name": "Shoa-Ting Yen",
                  "url": "https://github.com/yen900611"
                }]
            }
        </script>
        ```
        
2. 增加靜態網頁 關於我們
    
    新增一頁`關於我們`，HTML程式碼可以直接從[Github](https://github.com/yen900611/TNFood_DJ) Readme 複製下來，這樣版面就有一定的好看程度。
    
3. 使用GZip
    
    Django 只要將`GZipMiddleware` 放在Middleware的第一個就可以了。
    
    ```python
    # mysite/setting.py
    MIDDLEWARE = [
        'django.middleware.gzip.GZipMiddleware',
        ...
    ]
    ```
    
    - 使用GZip前 before
        
        ![before-gzip.png](Day30%20%E9%9C%B2%E5%87%BA%E6%B0%B4%E9%9D%A2%20SEO%204bc12eaff9704dbf839e66b8245dc96a/before-gzip.png)
        
    - 使用GZip後 after  可以看到API 封包容量大幅的縮小
        
        ![after-gzip.png](Day30%20%E9%9C%B2%E5%87%BA%E6%B0%B4%E9%9D%A2%20SEO%204bc12eaff9704dbf839e66b8245dc96a/after-gzip.png)
        
4. 提交站點地圖到[Google Search Console(GSC)](https://search.google.com/search-console/)
    
    這個動作可以請Google 引擎特別認識我們的網站。
    
    ![gsc-00.png](Day30%20%E9%9C%B2%E5%87%BA%E6%B0%B4%E9%9D%A2%20SEO%204bc12eaff9704dbf839e66b8245dc96a/gsc-00.png)
    
    ![gsc-01.png](Day30%20%E9%9C%B2%E5%87%BA%E6%B0%B4%E9%9D%A2%20SEO%204bc12eaff9704dbf839e66b8245dc96a/gsc-01.png)
    
    ![gsc-02.png](Day30%20%E9%9C%B2%E5%87%BA%E6%B0%B4%E9%9D%A2%20SEO%204bc12eaff9704dbf839e66b8245dc96a/gsc-02.png)
    

---

# 🛣️ 心得、結語

<aside>
💡 當然上面只會優化搜尋的結果，並沒辦法保證一定在第一個，畢竟結果的排名也是動態的。更何況每個人搜尋同一組關鍵字，出現的結果與排名更是不盡相同。

終於完成這30天的挑戰，我們可以浮出水面，雖然目前的店家還很少，之後這個網站會持續新增店家，有興趣的舊雨新知就不定時回來看看吧，感謝各位。

</aside>

# 參考資料

1. SEO 參考資料
    - [免費領取SEO檢查表](https://domyweb.org/seo-check-list/)
    - [SPA 對 SEO 不好？為了 SEO 所以要捨棄 CSR ? 多了解一點原理答案就自然浮現了！](https://progressbar.tw/posts/297)
    - [https://developers.google.com/search/docs/beginner/seo-starter-guide?hl=zh-tw](https://developers.google.com/search/docs/beginner/seo-starter-guide?hl=zh-tw)
    - [Google 搜尋引擎最佳化 (SEO) 入門指南](https://developers.google.com/search/docs/beginner/seo-starter-guide?hl=zh-tw)
2. SEO 攻擊
    - [三招對付惡意網頁點擊機器人](https://www.iware.com.tw/blog-601.html)
    - [網站遭到黑帽SEO 搜尋連結攻擊？一次了解他們的做法及防範](https://www.bnext.com.tw/article/69467/seo-newera)
    - [機器人的絕地大反攻 2020惡意網路機器人攻擊報告](https://www.ithome.com.tw/pr/138523)
3. Django SEO 
    - [https://github.com/whyflyru/django-seo](https://github.com/whyflyru/django-seo)
    

---

> **台南不需要米其林**
> 
> 1. 🏠[專案網址](https://tnfood.pythonanywhere.com/food/)
> 2. 🧑‍💻[專案程式碼](https://github.com/yen900611/TNFood_DJ) 
> 3. 📁[專案文件與鐵人賽文章](https://github.com/yen900611/TNFood)
> 4. 👥參賽團隊 ****[台南巷弄美食獵人](https://ithelp.ithome.com.tw/2022ironman/signup/team/256)****

---