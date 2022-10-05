# Day20 前後端分離最後一哩路 前端頁面實作

editor: 麒麟
tag: API, JQuery, django, django-ninja, 前後端分離
進度: 🌕

# 🏁 摘要

<aside>
💡 前兩天已經實作初步的API，讓我們可以取得店家的資料，今天要實作前後端分離的前端頁面。本次實作不會使用前端框架，而是使用原生JS語法和JQuery，來調整我們的介面。

</aside>

---

# 💡概念說明

## 前後端概念

- 前端指的是網頁瀏覽器端，給使用者看到的服務。
- 後端指的是網站伺服器端，真正實作系統功能的地方，負責抓取資料、整理資料、邏輯運算等功能。

## 為何要前後端分離？

1. 前後端分離的需求是因為，開發過程中，一旦後端邏輯資料調整更新，前端也需要因此改變。**任意一端沒有完成時，我們並不知道實作的成果是否正確**，因此無形中增加許多成本，這點在Django 上的模板機制完全展現。
2. 前兩天也有提到，再次強調一下。
前後端分離之後，透過API來交換資料，API會定義好前後端資料交換的格式與路徑，這樣一來後端邏輯增加時，可以單獨測試功能是否正確。前端也可以在準備好之後，再使用新製作的API。

## 前後端分離的方法

- 一般來說會將前端的程式碼部署在另外一台機器上，並且使用前端框架（像是Vue、Angular、React等）來開發，可以加快速度。
- 因為筆者想使用目前的域名，因此本次專案會將前端程式碼跟Django 都放在一起，但是網頁不接受 Django 傳進來的任何資料，而是使用JS原生的 `fetch` 方法來存取API，取得資料後再更新畫面資訊。
系統結構大致如下圖。
    
    ![Untitled](Day20%20%E5%89%8D%E5%BE%8C%E7%AB%AF%E5%88%86%E9%9B%A2%E6%9C%80%E5%BE%8C%E4%B8%80%E5%93%A9%E8%B7%AF%20%E5%89%8D%E7%AB%AF%E9%A0%81%E9%9D%A2%E5%AF%A6%E4%BD%9C%2049f1e50f988d481881b9a11b0402642b/Untitled.png)
    
- 前後端分離常會遇到所謂 CORS 的錯誤。API 一但開放之後，就會有各種安全性問題，可以參考下方文章解決。
    - [https://ithelp.ithome.com.tw/articles/10234669](https://ithelp.ithome.com.tw/articles/10234669)
- JS Fetch 概念
    - [https://www.casper.tw/javascript/2017/12/28/javascript-fetch/](https://www.casper.tw/javascript/2017/12/28/javascript-fetch/)
    - [https://ithelp.ithome.com.tw/articles/10252941](https://ithelp.ithome.com.tw/articles/10252941)
- JQuery 概念
    - [https://tw.alphacamp.co/blog/jquery-javascript-library-overview](https://tw.alphacamp.co/blog/jquery-javascript-library-overview)
    - [https://ithelp.ithome.com.tw/articles/10092592](https://ithelp.ithome.com.tw/articles/10092592)

---

# 🌟本日成果

![Untitled](Day20%20%E5%89%8D%E5%BE%8C%E7%AB%AF%E5%88%86%E9%9B%A2%E6%9C%80%E5%BE%8C%E4%B8%80%E5%93%A9%E8%B7%AF%20%E5%89%8D%E7%AB%AF%E9%A0%81%E9%9D%A2%E5%AF%A6%E4%BD%9C%2049f1e50f988d481881b9a11b0402642b/Untitled%201.png)

1. 調整URL
    - 因為仍會藉由Django來提供前端的程式碼，**只是不提供任何的資料**，因此調整`url.py`如下。
        
        ```python
        # food/urls.py
        def index(request):
            return render(request, 'food/index.html')
        
        def place_introduction(request,place_id:int):
            return render(request,'food/place_introduction.html')
        
        urlpatterns = [
            path('', index, name='food_index'),
            path('<int:place_id>/', place_introduction, name='food_introduction'),
        ]
        ```
        
2. 在`index.html`最底部嵌入一段JS程式碼
    - 主要就是使用fetch取得資料後，再使用JQuery 的套件，將HTML程式碼嵌入到畫面中，這樣就能繪出我們的首頁。
        
        ```html
        {% block custom_script %}
          <script>
              let place_url = `${API_ENDPOINT}/places`
              fetch(place_url)
                  .then(response => response.json())
                  .then(result => {
        // 這裡表示已經取得資料，須在這裡使用JQuery 繪製網頁
        })
        </script>
        {% endblock %}
        ```
        
    - 原本的`Django` `url view template model` 的 架構圖會變成下方的樣子，將template 和model完全切開來。
        
        ![Untitled](Day20%20%E5%89%8D%E5%BE%8C%E7%AB%AF%E5%88%86%E9%9B%A2%E6%9C%80%E5%BE%8C%E4%B8%80%E5%93%A9%E8%B7%AF%20%E5%89%8D%E7%AB%AF%E9%A0%81%E9%9D%A2%E5%AF%A6%E4%BD%9C%2049f1e50f988d481881b9a11b0402642b/Untitled%202.png)
        
    - fetch API 出錯的話，可以點選F12 看是否有錯誤訊息，很容易會有路徑的問題。
    - CORS 解決方式參考
        - [https://ithelp.ithome.com.tw/articles/10234669](https://ithelp.ithome.com.tw/articles/10234669)
3. 要注意網址列的問題，本地開發會使用`127.0.0.1:8000`，但是部署到伺服器上，網址就不同了，因此在使用到API、圖片的網址都需要特別注意。
可以使用下方程式來抓到主機位址，這樣就使用同一套程式碼即可在本地與伺服器運行了。
    
    ```jsx
    
    const API_HOST = window.location.protocol + "//" + window.location.host;
    const PATH = window.location.pathname
    const API_ENDPOINT = `${API_HOST}/api`
    ```
    
    - [https://pjchender.blogspot.com/2018/08/js-javascript-url-parameters.html](https://pjchender.blogspot.com/2018/08/js-javascript-url-parameters.html)

---

# 🛣️ 心得

<aside>
💡 雖然使用比較土炮的方式，但終於完成了前後端分離，可以更加敏捷的開發新的功能，明天就要邁入最後一個階段了，讓我們繼續努力。

</aside>

# 參考資料

1. CORS 
    - [https://ithelp.ithome.com.tw/articles/10234669](https://ithelp.ithome.com.tw/articles/10234669)
2. JS Fetch 
    - [https://www.casper.tw/javascript/2017/12/28/javascript-fetch/](https://www.casper.tw/javascript/2017/12/28/javascript-fetch/)
    - [https://ithelp.ithome.com.tw/articles/10252941](https://ithelp.ithome.com.tw/articles/10252941)
3. JQuery 
    - [https://tw.alphacamp.co/blog/jquery-javascript-library-overview](https://tw.alphacamp.co/blog/jquery-javascript-library-overview)
    - [https://ithelp.ithome.com.tw/articles/10092592](https://ithelp.ithome.com.tw/articles/10092592)
4. JavaScript 路徑
    - [https://pjchender.blogspot.com/2018/08/js-javascript-url-parameters.html](https://pjchender.blogspot.com/2018/08/js-javascript-url-parameters.html)

---

> **台南不需要米其林**
> 
> 1. 🏠[專案網址](https://tnfood.pythonanywhere.com/food/)
> 2. 🧑‍💻[專案程式碼](https://github.com/yen900611/TNFood_DJ) 
> 3. 📁[專案文件與鐵人賽文章](https://github.com/yen900611/TNFood)
> 4. 👥參賽團隊 ****[台南巷弄美食獵人](https://ithelp.ithome.com.tw/2022ironman/signup/team/256)****

---