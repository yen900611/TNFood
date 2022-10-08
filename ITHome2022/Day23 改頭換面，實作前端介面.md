# Day23 改頭換面，實作前端介面

editor: 麒麟
tag: API, django, front-end
進度: 🌕

# 🏁 前言、摘要

<aside>
💡 昨天已經將一些API 修復，因此今天將會依照著設計圖，調整我們前端的頁面。會使用到初階的Bootstrap和一些Django的模板語言。

</aside>

---

# 💡概念說明

1. 可以將頁面分成四個區塊，`header`、`熱門分類區`、`篩選器`、`結果顯示區`。
2. 因為我們使用Django模板引擎的輔助，可以輕易地將數個區塊分開編寫HTML程式。
3. 篩選器和結果顯示區，跟上一版的功能差不多，只是版面顯示有點不同。

![home_page_analysis.png](Day23%20%E6%94%B9%E9%A0%AD%E6%8F%9B%E9%9D%A2%EF%BC%8C%E5%AF%A6%E4%BD%9C%E5%89%8D%E7%AB%AF%E4%BB%8B%E9%9D%A2%20bf48f37d75a346528a52e3b28fc21cb8/home_page_analysis.png)

---

# 🌟本日成果

![Untitled](Day23%20%E6%94%B9%E9%A0%AD%E6%8F%9B%E9%9D%A2%EF%BC%8C%E5%AF%A6%E4%BD%9C%E5%89%8D%E7%AB%AF%E4%BB%8B%E9%9D%A2%20bf48f37d75a346528a52e3b28fc21cb8/Untitled.png)

1. 切出版型
    - 先稍微將畫面切成幾個區域，這樣就能局部實作。
    
    ![Untitled](Day23%20%E6%94%B9%E9%A0%AD%E6%8F%9B%E9%9D%A2%EF%BC%8C%E5%AF%A6%E4%BD%9C%E5%89%8D%E7%AB%AF%E4%BB%8B%E9%9D%A2%20bf48f37d75a346528a52e3b28fc21cb8/Untitled%201.png)
    
2. 局部實作
    - 主要使用Bootstrap 調整版面，熱門分類區的API尚未實作完畢，因此先預設好要顯示的版面。
    
    ![Untitled](Day23%20%E6%94%B9%E9%A0%AD%E6%8F%9B%E9%9D%A2%EF%BC%8C%E5%AF%A6%E4%BD%9C%E5%89%8D%E7%AB%AF%E4%BB%8B%E9%9D%A2%20bf48f37d75a346528a52e3b28fc21cb8/Untitled%202.png)
    
3. 修復API
    - 昨天修復了過濾器的功能，只要在取得`Place`的URL中後面加上查詢參數即可。
    - JS中可以使用`window.location.search`取得網頁的query 參數
        
        ```jsx
        let query_params = window.location.search
        let place_url = `${API_ENDPOINT}/places${query_params}`
        
        ```
        
4. 優化 css 檔案
    - 我們這週實作過程加入很多css 語法，若是全部嵌入在style之中，會增加網頁原始碼大小。
    - 因此可以存在一個css檔案中，變成`static`靜態檔案後，通常瀏覽器會有快取機制，因此可以減輕伺服器傳輸資料的流量。
    - 可以使用Django中模板語言，幫我們嵌入任何的靜態檔
        
        ```html
        {% load static %}
        <link rel="stylesheet" href="{% static 'food/css/base.css' %}">
            <link rel="stylesheet" href="{% static 'food/css/header.css' %}">
        ```
        

---

# 🛣️ 心得、結語

<aside>
💡 今天花一番功夫將畫面重新翻修了一次，也多饋有進行前後端分離，可以讓前端專注在調整畫面上，減少對後端資料變數名稱等等的依賴性。

</aside>

---

> **台南不需要米其林**
> 
> 1. 🏠[專案網址](https://tnfood.pythonanywhere.com/food/)
> 2. 🧑‍💻[專案程式碼](https://github.com/yen900611/TNFood_DJ) 
> 3. 📁[專案文件與鐵人賽文章](https://github.com/yen900611/TNFood)
> 4. 👥參賽團隊 ****[台南巷弄美食獵人](https://ithelp.ithome.com.tw/2022ironman/signup/team/256)****

---