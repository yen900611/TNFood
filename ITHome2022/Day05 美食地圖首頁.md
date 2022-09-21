# Day05 美食地圖首頁

進度: 🌕

# 🏁 前言

<aside>
💡 工欲善其事，必先利其器。花費一番心力，將環境與開發模式都弄好後，可以心無旁騖開始建立首頁，今天會一口氣畫好首頁的版面。

</aside>

---

# 💡概念說明

![Untitled](Day05%20%E7%BE%8E%E9%A3%9F%E5%9C%B0%E5%9C%96%E9%A6%96%E9%A0%81%202b5c3cd213cd463ebc6b484964d26070/Untitled.png)

# Django的設計理念與開發架構

Django採用的是MTV框架，由Model封裝跟處理資料庫、View負責處理 URL 與 callback 函式之間的關係，Template用於生成展示給User看的頁面，也就是前端 的HTML、CSS以及JavaScript。

## URL

URL是網站上的路徑，也可以說是瀏覽器與View之間的連結，不同的URL需要對應到不同的view function，當Django收到新的request的時候，才會知道要呼叫哪一個view function，這種對應關係就是URL configuration。

## View

當Django收到request後，會將其中的資訊封裝成一個HttpRequest，並根據上面提到的URL conf將Http request傳送給對應的view function。

當View function將這個request處理完後就會由View傳送HttpResponse給使用者，這個HttpResponse可能是一個網頁，一個重新導向(redirect)，或是一個404 error，或是圖片等等。

在View function可以將資料傳給Template渲染出網頁，然後再將生成的網頁送回瀏覽器中。

- View function 的規範
    - input 一定要有一個request變數，此變數會包含`HTTP request` 資訊
    - output 一定要是`HTTP Response`物件
    `render()`，則是可以把資料傳給template，把網頁畫面生成後，變成一個`HTTP Response`

## Template

本質上就是html語法，只是Django 有定義自己的模板語法，協助開發者簡化html中的元素。在PHP 撰寫的Laravel框架中也有類似的手法，使用blade引擎來作為網頁的模板。

最大的優勢，將網頁元件模組化，並重複使用。

- 像是一個網站中 header footer 是全體共用的，模組化後，只要在模板檔案中匯入header footer的模板即可。

Django簡易架構圖

![Untitled Diagram.drawio.png](Day05%20%E7%BE%8E%E9%A3%9F%E5%9C%B0%E5%9C%96%E9%A6%96%E9%A0%81%202b5c3cd213cd463ebc6b484964d26070/Untitled_Diagram.drawio.png)

---

# 🌟本日實作

- 確認好專案需求之後，就可以開始動手實作。因為網站包含了`前端`頁面開發和`後端`資料，今日就先專注在`前端`的頁面開發。

1. 開始一個專案
    - 在Django 之中，每個模組(app)，都會有獨立的專案資料夾。使用下方的指令可以直接建立一個模組，並自動產生一些檔案。
        
        ```bash
        python manage.py startapp food
        ```
        
    - 新增app後，要記得將`food`加入settings.py 的變數中，
    這非常重要，常常容易忘記
    因為Django 是一個很完整的框架，許多功能都是設定變數來啟動的，因此安裝套件時，大多都需要在這裡新增各式各樣的app，才能順利啟用。
        
        ```bash
        # mysite/settings.py
        INSTALLED_APPS = [
            'django.contrib.admin',
            'django.contrib.auth',
            'django.contrib.contenttypes',
            'django.contrib.sessions',
            'django.contrib.messages',
            'django.contrib.staticfiles',
            'food'
        ]
        ```
        
2. 更新路徑，手動增加一個 `urls.py`
    - 參考上述的概念，我們先改動`mysite/urls.py`的路徑，使用`include()`可以將`food/urls.py`的路徑啟用。
        
        ```python
        # mysite/urls.py
        def index(request):
            return HttpResponse("Hello World!")
        
        urlpatterns = [
        
            path('admin/', admin.site.urls),
            path('food/', include('food.urls')),
            path('', index)
        ]
        ```
        
    - 在`food`資料夾中新增一個`urls.py`，之後 food 模組的路徑就統一在這個檔案中管理。
    後面`index`是一個函式，就是我們下一步實作的函式，這種手法就是****依賴注入 Dependency Injection(DI)，****這種方法可以方便我們抽換函式，來改變此`url`要導向的地方。
        
        ```python
        # food/urls.py
        from .views import index
        
        urlpatterns = [
            path('', index),
        ]
        ```
        
3. 新增index()
    - 在 `view.py` 新增下方程式碼，使用`render()`這個function可以使用模板渲染出網頁。資料則是將字典丟入模板中，模板可直接使用`place_nums`這個變數。
        
        ```python
        def index(request):
            # return HttpResponse("Hello food!")
            return render(request, 'food/index.html',{'place_nums': range(6))
        ```
        
4. 新增 `index.html`  
    - 在`food`中新增兩層 `templates/food/` 資料夾，並建立一個`index.html`，若是前幾步沒有做錯，就會顯示初步的首頁囉。
        
        ![food_index_in_template.png](Day05%20%E7%BE%8E%E9%A3%9F%E5%9C%B0%E5%9C%96%E9%A6%96%E9%A0%81%202b5c3cd213cd463ebc6b484964d26070/food_index_in_template.png)
        
    - Django 會自動匯入此資料夾的模板檔案，詳情可以參考下方文章。
        
        > Your project’s **`[TEMPLATES](https://docs.djangoproject.com/en/4.1/ref/settings/#std-setting-TEMPLATES)`** setting describes how Django will load and render templates. The default settings file configures a **`DjangoTemplates`** backend whose **`[APP_DIRS](https://docs.djangoproject.com/en/4.1/ref/settings/#std-setting-TEMPLATES-APP_DIRS)`** option is set to **`True`**. By convention **`DjangoTemplates`** looks for a “templates” subdirectory in each of the **`[INSTALLED_APPS](https://docs.djangoproject.com/en/4.1/ref/settings/#std-setting-INSTALLED_APPS)`**.
        > 
        > 
        > [https://docs.djangoproject.com/en/3.2/intro/tutorial03/](https://docs.djangoproject.com/en/3.2/intro/tutorial03/)
        > 
    
5. 加上bootstrap css
    
    使用bootstrap可以快速讓網頁有一定的美觀程度。
    
    ![美化後](Day05%20%E7%BE%8E%E9%A3%9F%E5%9C%B0%E5%9C%96%E9%A6%96%E9%A0%81%202b5c3cd213cd463ebc6b484964d26070/food_index_with_style.png)
    
    美化後
    
6. 使用模板語言
    - Django 顯示前端網頁是使用自定義的模板語言， 在`{{ }}` 與 `{% %}`之間會被Django解析，其中包含了很多特殊功能，藉以產生完整的html文件。好處是同樣的元素，我們不用寫很多次，只需要寫一次，並搭配for迴圈即可，如下所示。
    
    ```html
    {% for i in place_nums %}
                  <div class="col-4 grid-item p-0 m-1">
                    <div class="grid-item-img">
                      <img src="https://picsum.photos/id/684/280/200" alt="">
                    </div>
                    <label for="">餐廳{{ i }}</label>
                  </div>
    {% endfor %}
    ```
    
    額外的語法可以參考：[https://docs.djangoproject.com/en/3.2/topics/templates/](https://docs.djangoproject.com/en/3.2/topics/templates/)
    

- 參考資料
    1. Django概念
        
        [https://djangogirlstaipei.gitbooks.io/django-girls-taipei-tutorial/content/django/views_and_urlconfs.html](https://djangogirlstaipei.gitbooks.io/django-girls-taipei-tutorial/content/django/views_and_urlconfs.html)
        
    2. Django模板語法
    [https://docs.djangoproject.com/en/3.2/topics/templates/](https://docs.djangoproject.com/en/3.2/topics/templates/)
    3. Bootstrap
        
        [https://getbootstrap.com/docs/4.6/getting-started/introduction/](https://getbootstrap.com/docs/4.6/getting-started/introduction/)
        
    4. 照片跑馬燈 [https://www.codeply.com/go/EIOtI7nkP8](https://www.codeply.com/go/EIOtI7nkP8)
    5. 如何產生特定尺寸的圖片、照片
        - [fakeimg](https://fakeimg.pl/)
        - [picsum](https://picsum.photos/)