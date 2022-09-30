# Day15 選出你喜歡的美味

editor: 劭庭
進度: 🌔

# 🏁 前言、摘要

<aside>
💡 建立好資料庫之後，我們今天要來完成初步的篩選器功能。
目標是藉由勾選左側的一個Tag，可以找到符合該條件的餐廳。

</aside>

---

# 💡概念說明

![Untitled](Day15%20%E9%81%B8%E5%87%BA%E4%BD%A0%E5%96%9C%E6%AD%A1%E7%9A%84%E7%BE%8E%E5%91%B3%200b376674d25c4551886bfef1bc5f488a/Untitled.png)

1. 過濾器是用表單來實作，按鈕按下就會送出一個HTTP GET的請求到Django伺服器上。
2. 我們會收到過濾器的條件，再去資料庫中，找出符合過濾器條件的餐廳，渲染完網頁之後再回傳。

---

# 🌟本日成果

首先我們觀察在`form_filter.html`裡面，會怎麼把勾選的內容存進Request

```html
<div class="form-check form-check-inline">
            <input class="form-check-input" type="checkbox" name="food_style" value={{tag.style}}>
            <label class="form-check-label" for="inlineCheckbox1">{{tag.name}}</label>
        </div>
```

這邊可以看`input`的部分，資料會以字典的形式存進Request中，`name`可以視為字典的key，`value`就是對應到的值。

### `view.py`

在`view.py`裡面找到`index`的function，我們會發現每次呼叫都會收到一個`request`，也就是網站的請求。這部分詳細的細節可以參考這篇文章：

[Request and response objects | Django documentation | Django](https://docs.djangoproject.com/en/4.1/ref/request-response/#django.http.QueryDict)

而現在我們只需要知道，當使用者按下搜尋鍵時，網站就回傳送一個新的`request`給view，請求新的資料，因此這個`request`是屬於`GET`。

在原有的function內加入以下程式碼

```python
try:
    if request.GET['food_style']:
         places = Place.objects.filter(tags__style=request.GET['food_style'])
except Exception:
        pass
```

這裡的`’food_style’`就是剛剛在`index.html`裡面看到的`name`，因此我們使用`filter`的功能，設定`tags__style`表示撈出每個`place`中`tags`的資料，如果有`tag的style`與`request`中的`value`相同，則回傳此間`place`。

![截圖 2022-09-30 下午10.20.40.png](Day15%20%E9%81%B8%E5%87%BA%E4%BD%A0%E5%96%9C%E6%AD%A1%E7%9A%84%E7%BE%8E%E5%91%B3%200b376674d25c4551886bfef1bc5f488a/%25E6%2588%25AA%25E5%259C%2596_2022-09-30_%25E4%25B8%258B%25E5%258D%258810.20.40.png)

---

# 🛣️ 心得、結語

<aside>
💡 Template中真的是滿滿的坑啊QAQ有機會把所有經歷過的災難整理成一篇文章給大家避雷。

</aside>

# 參考資料

[Request and response objects | Django documentation | Django](https://docs.djangoproject.com/en/4.1/ref/request-response/#django.http.QueryDict)

---

> **台南不需要米其林**
> 
> 1. 🏠[專案網址](https://tnfood.pythonanywhere.com/food/)
> 2. 🧑‍💻[專案程式碼](https://github.com/yen900611/TNFood_DJ) 
> 3. 📁[專案文件與鐵人賽文章](https://github.com/yen900611/TNFood)
> 4. 👥參賽團隊 ****[台南巷弄美食獵人](https://ithelp.ithome.com.tw/2022ironman/signup/team/256)****

---