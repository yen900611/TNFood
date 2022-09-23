# Day08 店家的詳細介紹-建立子頁

editor: 劭庭, 麒麟
進度: 🌔

# 🏁 前言、摘要

<aside>
💡 今日要實作餐廳介紹的子頁面，因此需要把\新增餐廳ORM的欄位。

</aside>

---

# 💡概念說明

![Untitled](Day08%20%E5%BA%97%E5%AE%B6%E7%9A%84%E8%A9%B3%E7%B4%B0%E4%BB%8B%E7%B4%B9-%E5%BB%BA%E7%AB%8B%E5%AD%90%E9%A0%81%20cea7ef72cd34448faece4b45a7d08330/Untitled.png)

---

### 新增URL

首先我們到`url.py`裡面新增一個pattern

```python
path('<int:place_id>/', place_introduction, name='food_introduction'),
```

接著在`view.py`的檔案裡面新增子頁面需要的資料。

今天我們會增加電話、官網以及店家介紹三種資料。

新建一個`place_introduction`的function，這個名稱要與剛剛的url pattern相同。因為目前只有店家的名字跟地址，因此等等我們在資料庫新增資料後再回來修改。

```python
def place_introduction(request, place_id: int):
    place = Place.objects.get(id=place_id)
    name = place.name
		address = place.address
		return render(
        request,
        'food/place_introduction.html',
        {'store': place, 'name': name, 'address': address,},
    )
```

### 新增子頁面Template

接下來我們進入到`template/food`的資料夾底下，建立名為`place_introduction.html`的檔案，也就是子頁面的template。

### 新增資料欄位

寫完HTML模板之後，我們回到後端編輯資料庫。

進入到`model.py`，我們預計在`Place`底下增加電話、官網等資料，所以在原有的程式碼底下加上這兩種新的欄位

```python
phone_number = models.CharField(max_length=20, default="No phone number")
web_site = models.CharField(max_length=200, default="No web site")
introduction = models.CharField(max_length=100)
```

這邊我們用到了`default`這項參數，因為並不是每家店都有電話或官網，所以設定一個預設值作為沒有這項資料時的內容。

改好的Model記得到Terminal使用以下兩個指令更新資料庫

```python
python manage.py makemigrations
python manage.py migrate
```

之後我們就可以執行`runserver`，到後台查看剛剛的更新

![截圖 2022-09-23 下午7.37.35.png](Day08%20%E5%BA%97%E5%AE%B6%E7%9A%84%E8%A9%B3%E7%B4%B0%E4%BB%8B%E7%B4%B9-%E5%BB%BA%E7%AB%8B%E5%AD%90%E9%A0%81%20cea7ef72cd34448faece4b45a7d08330/%25E6%2588%25AA%25E5%259C%2596_2022-09-23_%25E4%25B8%258B%25E5%258D%25887.37.35.png)

這邊我們也在資料庫中新增第二筆資料。櫻之庭食堂位於成大醫學院附近的巷弄內，是一間人氣極高的日式定食餐廳，其平易近人的價格與清淡少油卻美味的調味方式，使其成為附近學生與居民的最愛。除了主菜與三份小菜之外，內用還可續白飯或小菜一份，屬於CP值極高的小店，運氣好的話也許你還會遇上可愛的店貓在門口迎接你喔～

### 將資料套入模板中

最後我們將剛剛新增的資料加入template，預覽今天的成果。

找到我們剛剛預留的電話我們剛剛預留的電話以及網址的位置，填入剛剛兩項資料的名稱

```html
<div><span class="material-icons">
              phone
            </span><label for="">{{ phone_number }}</label></div>
          <div><span class="material-icons">
              open_in_new
            </span><label for="">{{ web_site }}</label></div>
```

還有店家介紹的欄位

```html
<div class="col-4">
          <label for="">店家簡介</label>
          <p>{{ introduction }}</p>
        </div>
```

# 🌟本日成果

![截圖 2022-09-23 下午7.40.35.png](Day08%20%E5%BA%97%E5%AE%B6%E7%9A%84%E8%A9%B3%E7%B4%B0%E4%BB%8B%E7%B4%B9-%E5%BB%BA%E7%AB%8B%E5%AD%90%E9%A0%81%20cea7ef72cd34448faece4b45a7d08330/%25E6%2588%25AA%25E5%259C%2596_2022-09-23_%25E4%25B8%258B%25E5%258D%25887.40.35.png)

![截圖 2022-09-23 下午7.40.22.png](Day08%20%E5%BA%97%E5%AE%B6%E7%9A%84%E8%A9%B3%E7%B4%B0%E4%BB%8B%E7%B4%B9-%E5%BB%BA%E7%AB%8B%E5%AD%90%E9%A0%81%20cea7ef72cd34448faece4b45a7d08330/%25E6%2588%25AA%25E5%259C%2596_2022-09-23_%25E4%25B8%258B%25E5%258D%25887.40.22.png)

---

# 參考資料

1. 

---

> **台南不需要米其林**
> 
> 1. [專案程式碼](https://github.com/yen900611/TNFood_DJ) 
> 2. [專案文件與鐵人賽文章](https://github.com/yen900611/TNFood)
> 3. 參賽團隊 ****[台南巷弄美食獵人](https://ithelp.ithome.com.tw/2022ironman/signup/team/256)****

---