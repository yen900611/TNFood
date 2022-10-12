# Day28 美食商店看板 使用Google 廣告

editor: 麒麟
tag: django, google Ads
進度: 🌕

# 🏁 前言、摘要

<aside>
💡 網站架設之後，長遠來看開銷也是相當可觀。考量成本，常見的方法是嵌入廣告，帶來額外的收入。因此今日會說明如何嵌入Google廣告。

</aside>

---

# 💡概念說明

1. 廣告的概念
    
    許多部落格平台，新聞網站都會在顯眼的地方嵌入廣告，以增加網站的收入。廣告投放者也會花錢競標熱門的網站與版面，增加自身服務的曝光度。
    
2. Google Adsense vs Google Ads
    - Google Adsense 是給開發者，網站平台方使用的服務，可以讓網站設定嵌入的廣告內容、位置。
    - Google Ads 是給廣告投放者，透過Google 協助投放自家廣告在什麼樣的網站上。
    
    ![ad_compare.png](Day28%20%E7%BE%8E%E9%A3%9F%E5%95%86%E5%BA%97%E7%9C%8B%E6%9D%BF%20%E4%BD%BF%E7%94%A8Google%20%E5%BB%A3%E5%91%8A%20a8d767907cff42c5b44a392dcf52c1b0/ad_compare.png)
    
    - 參考官方文件
        
        [https://support.google.com/adsense/answer/76231?hl=zh-Hant](https://support.google.com/adsense/answer/76231?hl=zh-Hant)
        
3. 需要考量的事項
    - 版面
        - 網站平台方會需要廣告，但也不會希望廣告喧賓奪主，影響到主要的服務。
    - 廣告內容
        - 廣告的內容是可以設定的，平台方可以決定不要嵌入特定主題的廣告內容。

---

# 🌟本日成果

![Untitled](Day28%20%E7%BE%8E%E9%A3%9F%E5%95%86%E5%BA%97%E7%9C%8B%E6%9D%BF%20%E4%BD%BF%E7%94%A8Google%20%E5%BB%A3%E5%91%8A%20a8d767907cff42c5b44a392dcf52c1b0/Untitled.png)

1. 申請Google Adsense 服務
    
    ![ad_start.png](Day28%20%E7%BE%8E%E9%A3%9F%E5%95%86%E5%BA%97%E7%9C%8B%E6%9D%BF%20%E4%BD%BF%E7%94%A8Google%20%E5%BB%A3%E5%91%8A%20a8d767907cff42c5b44a392dcf52c1b0/ad_start.png)
    
2. 輸入基本資訊
    
    建立完之後，可以依照畫面上的提示，設定付款資訊。
    
    ![ad_dashboard.png](Day28%20%E7%BE%8E%E9%A3%9F%E5%95%86%E5%BA%97%E7%9C%8B%E6%9D%BF%20%E4%BD%BF%E7%94%A8Google%20%E5%BB%A3%E5%91%8A%20a8d767907cff42c5b44a392dcf52c1b0/ad_dashboard.png)
    
3. 設定廣告限制
    
    左邊可以讓平台維運者選擇過濾掉哪些廣告。
    
    - 敏感類別
        
        ![ad_limit_category.png](Day28%20%E7%BE%8E%E9%A3%9F%E5%95%86%E5%BA%97%E7%9C%8B%E6%9D%BF%20%E4%BD%BF%E7%94%A8Google%20%E5%BB%A3%E5%91%8A%20a8d767907cff42c5b44a392dcf52c1b0/ad_limit_category.png)
        
    - 一般類別
        
        ![ad_category.png](Day28%20%E7%BE%8E%E9%A3%9F%E5%95%86%E5%BA%97%E7%9C%8B%E6%9D%BF%20%E4%BD%BF%E7%94%A8Google%20%E5%BB%A3%E5%91%8A%20a8d767907cff42c5b44a392dcf52c1b0/ad_category.png)
        
    
4. 嵌入程式碼
    - 自動廣告
    這樣Google 會自動在網站上嵌入廣告，可能會影響到版面。
    - 另外也可以自行嵌入程式碼在特定區域。
        
        ![ad_choose_type.png](Day28%20%E7%BE%8E%E9%A3%9F%E5%95%86%E5%BA%97%E7%9C%8B%E6%9D%BF%20%E4%BD%BF%E7%94%A8Google%20%E5%BB%A3%E5%91%8A%20a8d767907cff42c5b44a392dcf52c1b0/ad_choose_type.png)
        
    - 嵌入程式碼在Footer
        
        ```html
        <div style="height: 200px">
          {% block footer %}
            <script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-7226655551293951"
                    crossorigin="anonymous"></script>
        
            <!-- Footer的廣告 -->
            <ins class="adsbygoogle"
                 style="display:block"
                 data-ad-client="ca-pub-7226655551293951"
                 data-ad-slot="5788882957"
                 data-ad-format="auto"
                 data-full-width-responsive="true"></ins>
            <script>
                (adsbygoogle = window.adsbygoogle || []).push({});
            </script>
          {% endblock %}
        </div>
        ```
        
    
    好像需要一段時間才會出現，到時候再來看看成果吧。
    

---

# 🛣️ 心得、結語

<aside>
💡 相信大部分的人都不太喜歡廣告，也有研究指出人類會自動忽視廣告。但反過來說，若是剛好投放到自己有需要的內容，這不是一件很棒的事情嗎？
這中間的平衡是ㄧ種藝術，本網站設置的廣告也會篩選，不能破壞使用者體驗。

</aside>

# 參考資料

1. 在Wordpress中嵌入廣告原始碼
    - [https://zanzan.tw/archives/15282](https://zanzan.tw/archives/15282)
2. 廣告版面
    - [https://blog.user.today/put-ad-in-web-design/](https://blog.user.today/put-ad-in-web-design/)
3. 【網站經營】新手部落格 Google 廣告收入分享，如何計算？
    
    [https://girl-travel.com/google-adsense-income-202008/](https://girl-travel.com/google-adsense-income-202008/)
    

---

> **台南不需要米其林**
> 
> 1. 🏠[專案網址](https://tnfood.pythonanywhere.com/food/)
> 2. 🧑‍💻[專案程式碼](https://github.com/yen900611/TNFood_DJ) 
> 3. 📁[專案文件與鐵人賽文章](https://github.com/yen900611/TNFood)
> 4. 👥參賽團隊 ****[台南巷弄美食獵人](https://ithelp.ithome.com.tw/2022ironman/signup/team/256)****

---