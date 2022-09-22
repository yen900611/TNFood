# Day07 美食的倉庫 建立檔案儲存機制

editor: 麒麟
進度: 🌕

# 🏁 摘要

<aside>
💡 昨天已經使用Django *ORM* 來描述一間店家 Place ，今日會先新增照片的ORM，讓我們可以從後台上傳照片，同時也會說明Django 如何管理的上傳的檔案。

</aside>

---

# 💡概念說明

![Untitled](Day07%20%E7%BE%8E%E9%A3%9F%E7%9A%84%E5%80%89%E5%BA%AB%20%E5%BB%BA%E7%AB%8B%E6%AA%94%E6%A1%88%E5%84%B2%E5%AD%98%E6%A9%9F%E5%88%B6%20eeac5977fb5b44c5ab3dab785cb25fcf/Untitled.png)

## 網站的檔案管理

- 一個網頁除了html語法外，常會包含 css js 檔案，另外也有多媒體元素包含音樂、圖片、影片等等，網站開發者需要找一個地方放置這些檔案。常見的方法有以下幾種
    - 伺服器上的資料夾
    最傳統的方式，將上述所有檔案放到自己管理的資料夾，再讓伺服器去讀取即可。
    - 使用內容傳遞網路（*CDN*）
    常見於受歡迎的 css js 框架，像是前幾天提到的Bootstrap ，可以減少自身伺服器的流量。
    缺點是如果本地開發，處於斷網狀態後，就會無法正常運作。
    - 雲端服務儲存體
    使用AWS Azure GCP等雲端服務的儲存空間，此方法可以減少管理伺服器磁碟空間的心力，存取雲端的檔案，不管是速度還是可靠度都較穩定。
- 除了網頁上會使用到的檔案，也會需要讓使用者上傳檔案
    - 檔案如何放置儲存、如何給使用者瀏覽、檔案的瀏覽權限等等都是需要注意的問題。
- Django 是一個很完整的框架，許多機制都很完善，基本上只需要設定一次即可，基本上都是在`settings.py`設定一些全域變數即可。
    
    > *為了解決這個問題，在Django中另外提供了一個叫做`media`資料夾的功能，在`settings.py`中做一些設定，就可以讓讓資料夾中的檔案可以即時地被網站中的程式加以存取利用，所以，我們就可以把上傳之後的檔案放到那個資料夾中。*
    > 
    > - 引用自 [https://104.es/2022/03/19/django-upload_files/](https://104.es/2022/03/19/django-upload_files/)
    - [https://docs.djangoproject.com/en/3.2/topics/files/](https://docs.djangoproject.com/en/3.2/topics/files/)

---

# 🌟本日成果

1. 新增Photo and Admin site
    
    新增新的ORM，還要再設定Admin 才可以在django管理者介面看到基本的管理介面。
    
    ```python
    # food/models.py
    class Photo(models.Model):
        name=models.CharField(max_length=255)
    		#照片我們會使用ImageField 這個欄位，upload_to 是表示檔案要儲存在哪個資料夾
        file=models.ImageField(upload_to='photos')
    ```
    
    ```python
    # food/admin.py
    from django.contrib import admin
    
    from .models import Photo
    from .models import Place
    
    admin.site.register(Photo, admin.ModelAdmin)
    admin.site.register(Place,admin.ModelAdmin)
    ```
    
2. 安裝pillow and migrate
    - ImageField 會需要安裝 pillow ，在requirements.txt 新增套件要記得安裝喔。
        
        ```python
        # requirements.txt
        Django==3.2
        Pillow==9.2.0
        ```
        
    - 建立好ORM 要Migrate 才會建立資料表
        
        ```bash
        pip install -r requirements.txt
        python manage.py makemigrations
        python manage.py migrate
        ```
        
    - 結果
        
        ![django_admin.png](Day07%20%E7%BE%8E%E9%A3%9F%E7%9A%84%E5%80%89%E5%BA%AB%20%E5%BB%BA%E7%AB%8B%E6%AA%94%E6%A1%88%E5%84%B2%E5%AD%98%E6%A9%9F%E5%88%B6%20eeac5977fb5b44c5ab3dab785cb25fcf/django_admin.png)
        
3. 設定全域變數來決定檔案上傳位置
    - 提供檔案給網頁渲染使用的如 css js  icon 圖片 聲音等，會設定 `STATIC_*` 的環境變數
    - 設定上傳檔案儲存位置的，會設定 `MEDIA_*`
    - `mysite/settings.py`
        
        ```python
        STATIC_URL = 'static/'
        STATIC_ROOT='static/'
        MEDIA_ROOT = Path.joinpath(BASE_DIR, 'media')
        MEDIA_URL = '/media/'
        ```
        
    - `mysite/urls.py`
        
        ```bash
        urlpatterns = \
            [
                path('admin/', admin.site.urls),
                path('food/', include('food.urls')),
                path('', index)
            ] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
        ```
        
4. 手動上傳圖片
    
    ![django_add_photo.png](Day07%20%E7%BE%8E%E9%A3%9F%E7%9A%84%E5%80%89%E5%BA%AB%20%E5%BB%BA%E7%AB%8B%E6%AA%94%E6%A1%88%E5%84%B2%E5%AD%98%E6%A9%9F%E5%88%B6%20eeac5977fb5b44c5ab3dab785cb25fcf/django_add_photo.png)
    
5. 將圖片傳至前端
    - 前端顯示圖片的方式是使用檔案儲存的網址，因此只要將照片的url 嵌入 img 元素即可。
    - `food/views.py`
        - 因為我們上傳了一張圖片，就直接使用`Photo.objects.first()` 來抓取第一張圖。
        
        ```python
        def index(request):
            photo = Photo.objects.first()
            places = Place.objects.all()
            return render(
                request,
                'food/index.html',
                {'store_list': places, 'photo': photo.file}
            )
        ```
        
    - `food/templates/food/index.html`
        - `Photo` 物件的 `file` 變數是 `ImageField` 型別，此型別的物件可以使用 `obj.url`取得網址。
            - [https://docs.djangoproject.com/en/3.2/ref/models/fields/#django.db.models.FileField](https://docs.djangoproject.com/en/3.2/ref/models/fields/#django.db.models.FileField)
        
        ```html
        {% for store in store_list %}
                      <div class="col-4 grid-item p-0 m-1">
                        <div class="grid-item-img">
                          <img style="max-width:280px" src="{{ photo.url }}" alt="">
                        </div>
                        <label for="">{{ store.name }}</label>
                      </div>
        
                    {% endfor %}
        ```
        
6. 顯示結果
    
    ![food_index_with_photo.png](Day07%20%E7%BE%8E%E9%A3%9F%E7%9A%84%E5%80%89%E5%BA%AB%20%E5%BB%BA%E7%AB%8B%E6%AA%94%E6%A1%88%E5%84%B2%E5%AD%98%E6%A9%9F%E5%88%B6%20eeac5977fb5b44c5ab3dab785cb25fcf/food_index_with_photo.png)
    

---

# 🛣️ 結語

<aside>
💡 檔案管理是一個很大的學問，目前我們的專案還沒有要正式上線，因此使用本地的資料夾即可，但是真正上線可能就需要使用雲端服務的儲存空間，Django 這方面設定也是相當簡單，有需要的讀者可以自行搜尋。

</aside>

---

# 參考資料

1. Django 檔案管理機制
    - [https://docs.djangoproject.com/en/3.2/topics/files/](https://docs.djangoproject.com/en/3.2/topics/files/)
    - [https://docs.djangoproject.com/en/3.2/howto/static-files/](https://docs.djangoproject.com/en/3.2/howto/static-files/)
    - [https://104.es/2022/03/19/django-upload_files/](https://104.es/2022/03/19/django-upload_files/)
2. Django如何上傳檔案
    - [https://www.learncodewithmike.com/2020/04/django-image-upload.html](https://www.learncodewithmike.com/2020/04/django-image-upload.html)

---

> **台南不需要米其林**
> 
> 1. [**專案程式碼](https://github.com/yen900611/TNFood_DJ)** 
> 2. [**專案文件與鐵人賽文章**](https://github.com/yen900611/TNFood)
> 3. 參賽團隊 ****[台南巷弄美食獵人](https://ithelp.ithome.com.tw/2022ironman/signup/team/256)****

---