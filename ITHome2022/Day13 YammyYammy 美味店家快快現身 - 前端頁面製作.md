# Day13 YammyYammy 美味店家快快現身 - 前端頁面製作

editor: 麒麟
進度: 🌕

# 🏁 前言

<aside>
💡 我們現在要開始實作篩選店家的機制，今天會先完成網頁的顯示介面，明天再繼續背後的邏輯實作。

</aside>

---

# 💡概念說明

![Untitled](Day13%20YammyYammy%20%E7%BE%8E%E5%91%B3%E5%BA%97%E5%AE%B6%E5%BF%AB%E5%BF%AB%E7%8F%BE%E8%BA%AB%20-%20%E5%89%8D%E7%AB%AF%E9%A0%81%E9%9D%A2%E8%A3%BD%E4%BD%9C%204c44afc5ecac43acbc410d9a605261ca/Untitled.png)

使用者操作的流程會是：在前端網頁，選好條件，按下按鈕，美味店家就要立刻現身。

- 使用表單輸入查詢條件
    
    HTML元素中有一個form的元素，就是我們常見的表單，可以讓使用者填寫一些資料送到後端的伺服器，跟伺服器做互動。
    
    不管表單內的元素有多少，形式樣式如何，表單最後都會將資料打包送給伺服器，送資料的方式有兩種。
    
    - `GET`：表單的資料會被放在網址列，沒有隱蔽性，但是可讀性高，因此多用於查詢取得資料。
    - `POST`：表單的資料會被壓縮加密放在封包之中，隱蔽性較高，因此多用於提交機密資訊或傳送新增的資料。
    
    過濾器就使用`GET`即可
    
- 使用 `Django` 模板語言特殊語法
    
    Django自帶的模板有許多特殊用法，善用的話可以增加模板的結構性，減少重複的程式碼。
    
    - `include`
        - 顧名思義就是在此模板中，注入另外一個檔案的`HTML`語法，很適合用此方法將網頁元素拆小模組化，並重複使用。
    - `extend`
        - 以一個`HTML`檔案為基礎進行擴展，習慣上，會將網站上大致的版型製作成模板 `base.html`，裡面包含每頁都相同的程式碼（header footer等）
        - `base.html` 中會有 `block` 關鍵字，表示預留的區塊。其他網站擴展`base.html`時，只需要填入`block`應該要有的程式碼，就能擴展成功。
    - 下列這篇文章有詳細的說明
        1. [http://dokelung-blog.logdown.com/posts/235711-django-notes-12-template-advanced-technique](http://dokelung-blog.logdown.com/posts/235711-django-notes-12-template-advanced-technique)
        

---

# 🌟本日成果

1. 使用Django 模板語言重構頁面
    - 模板 `base.html`
        
        可以看到程式碼中 有`block` `endblock`  區塊，就是我們預留的區塊，其他檔案要使用base.html 作為基礎，只需要填入 block 區間即可。
        
        ```html
        <-- food/base.html -->
        <!doctype html>
        <html>
        <head>
          <title>{% block title %}{% endblock %}</title>
          <meta charset="utf-8">
          {% **block** style %}
            <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">
            <link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
            <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/css/bootstrap.min.css"
                  integrity="sha384-TX8t27EcRE3e/ihU7zmQxVncDAy5uIKz4rEkgIXeMed4M0jlfIDPvg6uqKI2xXr2" crossorigin="anonymous">
            
            </style>
          {% **endblock** %}
          {% block custom_style %}{% endblock %}
        </head>
        <body>
        <header>
          <!-- search block -->
          <div class="p-3">
        
            <div class="m-auto mt-3">
              <!-- The form -->
              <a href="/food" class="btn inline"><h2>台南不需要米其林</h2></a>
        
            </div>
          </div>
        </header>
        
        <main>
          {% block main %}{% endblock %}
        </main>
        <footer>
        
        </footer>
        </body>
        {% block script %}
        
          <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js"
                  integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj"
                  crossorigin="anonymous"></script>
          <script src="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/js/bootstrap.bundle.min.js"
                  integrity="sha384-ho+j7jyWK8fNQe+A12Hb8AhRq26LrZ/JpcUGGOn+Y7RsweNrtN/tE3MoK7ZeZDyx"
                  crossorigin="anonymous"></script>
        {% endblock %}
        {% block custom_script %}
        {% endblock %}
        </html>
        ```
        
    - 重構 `index.html`
        
        開頭有使用`extends` 表示要從哪個檔案開始擴展，在`block` `endblock` 之間填入的程式碼就會直接抽換掉`base.html`同樣block之間的程式碼。
        
        ```html
        {% extends 'food/base.html' %}
        
        {% block title %} 台南不需要米其林 {% endblock %}
        {% block custom_style %}
          <style>
        
              .template-sidebar {
                  min-height: 600px;
                  background-color: #FFFFFF;
                  border: #AE3338 5px solid;
              }
          </style>
        {% endblock %}
        {% block main %}
        
          <div class="container-fluid border-bottom">
            <div class="row ">
              <div class="col-3 border-right template-sidebar m-1">
              {% include 'food/form_filter.html' %}
              </div>
              <div class="col-8">
                <div class="container-fluid">
                  <div class="row ">
                    {% for store in store_list %}
                      {# 使用模板語言 嵌入URL #}
                      <a href="{% url 'food_introduction' place_id=store.id %}">
        
                        <div class="col-4 grid-item p-0 m-1">
                          <div class="grid-item-img">
                            <img class="p-1" style="max-width:280px;max-height: 200px" src="{{ photo.url }}" alt="">
                          </div>
                          <label for="">{{ store.name }}</label>
                        </div>
                      </a>
        
                    {% endfor %}
                  </div>
                </div>
              </div>
            </div>
          </div>{% endblock %}
        ```
        
2. 新增過濾器 `form_filter`
    - 在上述的程式碼中，我在中間嵌入`{% include 'food/form_filter.html' %}`這樣可以直接將`'food/form_filter.html'`注入到這個區塊。
    - 有興趣可以直接到[github](https://github.com/yen900611/TNFood_DJ/blob/0.1.3/mysite/food/templates/food/form_filter.html) 上查詢
    
    ![index.png](Day13%20YammyYammy%20%E7%BE%8E%E5%91%B3%E5%BA%97%E5%AE%B6%E5%BF%AB%E5%BF%AB%E7%8F%BE%E8%BA%AB%20-%20%E5%89%8D%E7%AB%AF%E9%A0%81%E9%9D%A2%E8%A3%BD%E4%BD%9C%204c44afc5ecac43acbc410d9a605261ca/index.png)
    
3. 更新子頁面的版型
    - 新增分類到頁面的中間，一樣有使用到bootstrap 的一些功能，若有興趣的人再去看吧～
        
        ![food.png](Day13%20YammyYammy%20%E7%BE%8E%E5%91%B3%E5%BA%97%E5%AE%B6%E5%BF%AB%E5%BF%AB%E7%8F%BE%E8%BA%AB%20-%20%E5%89%8D%E7%AB%AF%E9%A0%81%E9%9D%A2%E8%A3%BD%E4%BD%9C%204c44afc5ecac43acbc410d9a605261ca/food.png)
        
    

---

# 🛣️ 結語

<aside>
💡 頁面逐漸完善，隨著店家愈來愈多，網站就會愈來愈豐富。
接下來兩天就會套上真實資料，敬請期待。

</aside>

# 參考資料

1. Django筆記(12) - 模板進階技巧
****[http://dokelung-blog.logdown.com/posts/235711-django-notes-12-template-advanced-technique](http://dokelung-blog.logdown.com/posts/235711-django-notes-12-template-advanced-technique)
    
    [https://docs.djangoproject.com/en/3.2/ref/templates/builtins/](https://docs.djangoproject.com/en/3.2/ref/templates/builtins/)
    

---

> **台南不需要米其林**
> 
> 1. 🏠[專案網址](https://tnfood.pythonanywhere.com/food/)
> 2. 🧑‍💻[專案程式碼](https://github.com/yen900611/TNFood_DJ) 
> 3. 📁[專案文件與鐵人賽文章](https://github.com/yen900611/TNFood)
> 4. 👥參賽團隊 ****[台南巷弄美食獵人](https://ithelp.ithome.com.tw/2022ironman/signup/team/256)****

---