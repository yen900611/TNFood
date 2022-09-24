# Day09 美食照片不可少 - 建立照片與店家的關係

# 🏁 前言、摘要

<aside>
💡 昨天已經新增了子頁面，之前是直接將照片嵌入在網頁中，並沒有考慮到店家與照片的關聯，今天要在子頁面上面放上對應店家的圖片。

</aside>

---

![Untitled](Day09%20%E7%BE%8E%E9%A3%9F%E7%85%A7%E7%89%87%E4%B8%8D%E5%8F%AF%E5%B0%91%20-%20%E5%BB%BA%E7%AB%8B%E7%85%A7%E7%89%87%E8%88%87%E5%BA%97%E5%AE%B6%E7%9A%84%E9%97%9C%E4%BF%82%20ccb0b9e562244e0a82901e5049f3a845/Untitled.png)

1. 更新 Photo ORM
    
    一間店會有很多照片，因此我們需要將 Place 跟 Photo 建立一對多的關係。
    
    在Django ORM的設計中，可以在一個物件中嵌入 Foreign Key 建立一對多的關係。
    
    - 程式碼
        
        ```python
        # food/model.py
        class Photo(models.Model):
            name = models.CharField(max_length=255)
            file = models.ImageField(upload_to='photos')
            place = models.ForeignKey(Place, help_text="The place that this photo come from.", on_delete=models.SET_NULL, null=True)
        ```
        
        Ｑ：餐廳若是關起來了，我上傳的照片會怎麼樣？
        Ａ：兩個資料有關聯時，其中一筆資料（舉例餐廳）被刪除時常見有兩種做法。
        
        1. 跟著刪除照片，是最常見的做法，可以節省資料庫的空間，但要注意是否有其他地方使用到這張照片，如果有的話會造成網頁顯示異常，另外就是要記得將資料夾中這張照片同時刪除，因為資料庫記錄的是這張照片的位置，不是照片的內容。
        2. 保留照片，是另一個保險的做法，可以將照片保留，避免無法意料的異常，但就會把這張照片設定為不屬於任何一個餐廳。
        - 中間有一個參數 on_delete=models.SET_NULL 就是表示 Place 被刪除時，不會刪除此Photo，只會設定為Null
        
        - [https://www.delftstack.com/zh-tw/howto/django/django-setup-one-to-many-relationship/](https://www.delftstack.com/zh-tw/howto/django/django-setup-one-to-many-relationship/)
        - [https://docs.djangoproject.com/en/3.2/topics/db/examples/many_to_one/](https://docs.djangoproject.com/en/3.2/topics/db/examples/many_to_one/)
    
    - 修改完一樣要記得 makemigrations 和 migrate
        
        ```bash
        python manage.py makemigrations
        python manage.py migrate
        ```
        
2. 將此店家的所有照片傳至模板
    
    有兩種方法可以達成這個任務
    
    1. 直接在Photo 資料表中，找出屬於這一間餐廳的照片，使用`filter()` 這個過濾器，裡面可以直接使用`place`這個參數 。
        
        `photo_list = Photo.objects.filter(place = place).all()`
        
    2. 從餐廳的角度去搜尋，找出這間餐廳的所有照片，
    因為Place 和 Photo 有關係，因此Django會自動幫Place產生photo_set這個變數，代表的就是這家餐廳的所有照片。
        
        `place.photo_set.all()`
        
        兩者的結果是一致的，只是思考思路和語法有所不同。
        
    
    ```python
    def place_introduction(request, place_id: int):
        place = Place.objects.get(id=place_id)
        # 方法一
        # photo_list = Photo.objects.filter(place = place).all()
        # 方法二
        photo_list = place.photo_set.all()
        return render(
            request,
            'food/place_introduction.html',
            {'store': place,
             'photo_list':photo_list,
             },
        )
    ```
    
3. integrate data 
    
    最後我們執行`runserver`進入`/admin`，將照片上傳到資料庫
    
    會發現在place的地方成為了選單，內容是之前新增的店家資料
    
    ![截圖 2022-09-24 下午10.48.20.png](Day09%20%E7%BE%8E%E9%A3%9F%E7%85%A7%E7%89%87%E4%B8%8D%E5%8F%AF%E5%B0%91%20-%20%E5%BB%BA%E7%AB%8B%E7%85%A7%E7%89%87%E8%88%87%E5%BA%97%E5%AE%B6%E7%9A%84%E9%97%9C%E4%BF%82%20ccb0b9e562244e0a82901e5049f3a845/%25E6%2588%25AA%25E5%259C%2596_2022-09-24_%25E4%25B8%258B%25E5%258D%258810.48.20.png)
    

# 🌟本日成果

![截圖 2022-09-24 下午10.49.39.png](Day09%20%E7%BE%8E%E9%A3%9F%E7%85%A7%E7%89%87%E4%B8%8D%E5%8F%AF%E5%B0%91%20-%20%E5%BB%BA%E7%AB%8B%E7%85%A7%E7%89%87%E8%88%87%E5%BA%97%E5%AE%B6%E7%9A%84%E9%97%9C%E4%BF%82%20ccb0b9e562244e0a82901e5049f3a845/%25E6%2588%25AA%25E5%259C%2596_2022-09-24_%25E4%25B8%258B%25E5%258D%258810.49.39.png)

---

# 參考資料

1. [https://www.delftstack.com/zh-tw/howto/django/django-setup-one-to-many-relationship/](https://www.delftstack.com/zh-tw/howto/django/django-setup-one-to-many-relationship/)
2. [Django Tutorial Part 3: Using models - 學習該如何開發 Web | MDN (mozilla.org)](https://developer.mozilla.org/zh-TW/docs/Learn/Server-side/Django/Models)

---

> **台南不需要米其林**
> 
> 1. [專案程式碼](https://github.com/yen900611/TNFood_DJ) 
> 2. [專案文件與鐵人賽文章](https://github.com/yen900611/TNFood)
> 3. 參賽團隊 ****[台南巷弄美食獵人](https://ithelp.ithome.com.tw/2022ironman/signup/team/256)****

---