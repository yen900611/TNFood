# Day29 根據需求重整資料庫

editor: 劭庭
tag: API, DB, admin, django

# 🏁 前言、摘要

<aside>
💡 在增加了資料庫的同時，我們發現原本放置Tag的排版跑掉了，因此我們將會對tag進行分類。既然都要修改資料庫了，我們就重新檢視一次目前的資料庫，看看有什麼要修改的，並繪製資料庫的結構圖，方便之後維護使用。

</aside>

---

# 💡概念說明

今天預計會將Tag分成三種類別：素別（`vegan_style`）、食物類型（`category`）、食物風格（`food_style`）

因此需要新增`group`這個欄位，並且設定固定的選項。

最後我們會把目前資料庫的內容會製成圖。

---

# 🌟本日成果

![Untitled](Day29%20%E6%A0%B9%E6%93%9A%E9%9C%80%E6%B1%82%E9%87%8D%E6%95%B4%E8%B3%87%E6%96%99%E5%BA%AB%206a5bf7be740244839b1a71450064514c/Untitled.png)

在新增了所有Tag之後，我們發現版面變成這樣：

![截圖 2022-10-14 上午1.15.23.png](Day29%20%E6%A0%B9%E6%93%9A%E9%9C%80%E6%B1%82%E9%87%8D%E6%95%B4%E8%B3%87%E6%96%99%E5%BA%AB%206a5bf7be740244839b1a71450064514c/%25E6%2588%25AA%25E5%259C%2596_2022-10-14_%25E4%25B8%258A%25E5%258D%25881.15.23.png)

因此我們要把Tag分成三個類型。

### 新增Tag欄位

我們將`Tag`改成以下程式碼：

```python
class Tag(models.Model):
    vegan_style = 'V'
    category = 'C'
    food_style = 'F'
    name = models.CharField(max_length=10)
    value = models.CharField(max_length=30, default="None")
    group = models.CharField(max_length=30, default=food_style, choices=[(vegan_style, 'vegan_style'),
                                                                         (category, 'category'),
                                                                         (food_style, 'food_style'), ])
```

這邊我們把之前的`style`改成`value`，更符合其功能。並新增`group`用來之後前端分類使用。

這邊我們使用了之前沒有用過的參數`choices`，使用這個參數的時候我們需要用list裝入選項，每個選項是一個tuple，包含兩項值，第一欄是Model中的實際值，第二欄則是開發者可讀的名稱。

記得如果有修改資料庫就需要在Terminal裡面執行以下兩個指令

```bash
python manage.py makemigrations
python manage.py migrate
```

打開admin就可以看到新的資料內容

![admin.png](Day29%20%E6%A0%B9%E6%93%9A%E9%9C%80%E6%B1%82%E9%87%8D%E6%95%B4%E8%B3%87%E6%96%99%E5%BA%AB%206a5bf7be740244839b1a71450064514c/admin.png)

最後我們把目前的資料庫內容會製成圖表：

![TNFood資料結構.drawio.png](Day29%20%E6%A0%B9%E6%93%9A%E9%9C%80%E6%B1%82%E9%87%8D%E6%95%B4%E8%B3%87%E6%96%99%E5%BA%AB%206a5bf7be740244839b1a71450064514c/TNFood%25E8%25B3%2587%25E6%2596%2599%25E7%25B5%2590%25E6%25A7%258B.drawio.png)

---

# 參考資料

1. [模型欄位參考|詹戈文檔|詹戈 (djangoproject.com)](https://docs.djangoproject.com/en/4.1/ref/models/fields/)

---

> **台南不需要米其林**
> 
> 1. 🏠[專案網址](https://tnfood.pythonanywhere.com/food/)
> 2. 🧑‍💻[專案程式碼](https://github.com/yen900611/TNFood_DJ) 
> 3. 📁[專案文件與鐵人賽文章](https://github.com/yen900611/TNFood)
> 4. 👥參賽團隊 ****[台南巷弄美食獵人](https://ithelp.ithome.com.tw/2022ironman/signup/team/256)****

---