# Day06 新增第一個美味店家 建立資料庫

editor: 劭庭
進度: 🌕

# 🏁 摘要

<aside>
💡 今日目標：手動建立店家的資料庫，包含店名與地址。
我們會從Django的Model及資料庫編輯開始談起，並進入後台管理、查看資料庫。

</aside>

---

# 💡概念說明

![Untitled](Day06%20%E6%96%B0%E5%A2%9E%E7%AC%AC%E4%B8%80%E5%80%8B%E7%BE%8E%E5%91%B3%E5%BA%97%E5%AE%B6%20%E5%BB%BA%E7%AB%8B%E8%B3%87%E6%96%99%E5%BA%AB%206fb9f6e050d44f28a5b57aa398bf9208/Untitled.png)

- 要教學如何新增簡易的admin頁面

---

### 創建Model

還記得在昨天的文章中我們有提到，Django框架中Model負責處理資料庫相關的事情。因此當我們要建立資料庫的時候，就在`model.py`的檔案裡面進行設定。

打開`model.py`，輸入以下程式碼：

```python
from django.db import models

class Place(models.Model):
    name = models.CharField(max_length=20, help_text='Enter the name of store')
    address = models.CharField(max_length=50, null=True)
    pub_date = models.DateTimeField('date published')
    def __str__(self):
        return self.name
```

![截圖 2022-09-20 下午9.13.52.png](Day06%20%E6%96%B0%E5%A2%9E%E7%AC%AC%E4%B8%80%E5%80%8B%E7%BE%8E%E5%91%B3%E5%BA%97%E5%AE%B6%20%E5%BB%BA%E7%AB%8B%E8%B3%87%E6%96%99%E5%BA%AB%206fb9f6e050d44f28a5b57aa398bf9208/%25E6%2588%25AA%25E5%259C%2596_2022-09-20_%25E4%25B8%258B%25E5%258D%25889.13.52.png)

這邊詳細解釋一下有使用到的Model的功能，因為目前是以每間店做為資料儲存的單位，因此我們建立一個名為`Place`的Model，用來儲存每間店的資料。

這裡面我希望儲存每間店的名稱跟地址，並且使用字串來儲存，所以我呼叫`models.CharField`這個Function，而後面使用到的參數包括：

- `max_length`：表示此一欄位所可以接受的最大字符長度。
- `help_text`：讓User可以知道這個欄位要填入什麼資料的說明文字。
- `null`：Bool值。預設為 `False`。如果設定成 `True`，就代表 資料庫中該欄位可以寫入 `NULL`。但如果欄位型態為 `CharField`則會變成空字串）。

其他的其他的參數選項可以參考[這裡](https://docs.djangoproject.com/en/1.10/ref/models/fields/#field-options)。

設定好之後我們把剛剛新增的Model寫進資料庫中，在Terminal連續輸入以下兩個指令：

```bash
python manage.py makemigrations
python manage.py migrate
```

看到ok就代表建立成功囉～

![截圖 2022-09-20 下午9.35.38.png](Day06%20%E6%96%B0%E5%A2%9E%E7%AC%AC%E4%B8%80%E5%80%8B%E7%BE%8E%E5%91%B3%E5%BA%97%E5%AE%B6%20%E5%BB%BA%E7%AB%8B%E8%B3%87%E6%96%99%E5%BA%AB%206fb9f6e050d44f28a5b57aa398bf9208/%25E6%2588%25AA%25E5%259C%2596_2022-09-20_%25E4%25B8%258B%25E5%258D%25889.35.38.png)

最後，因為我們要透過admin的網站方便管理我們的資料庫，所以，所以也要讓網站上面新增物件。

打開`admin.py`，import剛剛新增的model之後建立它的欄位。

```python
from .models import Place

admin.site.register(Place)
```

### 創建管理員（admin）

為了管理後端的資料庫，我們要創建一個擁有管理員權限的User，在Terminal中輸入以下指令

```bash
python manage.py createsuperuser
```

系統會詢問你想要使用的username，輸入`admin`後按下Enter

![截圖 2022-09-20 下午8.11.35.png](Day06%20%E6%96%B0%E5%A2%9E%E7%AC%AC%E4%B8%80%E5%80%8B%E7%BE%8E%E5%91%B3%E5%BA%97%E5%AE%B6%20%E5%BB%BA%E7%AB%8B%E8%B3%87%E6%96%99%E5%BA%AB%206fb9f6e050d44f28a5b57aa398bf9208/%25E6%2588%25AA%25E5%259C%2596_2022-09-20_%25E4%25B8%258B%25E5%258D%25888.11.35.png)

接著輸入自己的email、設定密碼後就完成superuser的新增了。

### 登入使用網站

為了進入到管理網站，我們為了進入到管理網站，我們奧先連上admin的URL，在Terminal使用runserver指令

```bash
python manage.py runserver
```

進入網站後，在原有的網址後方加上`/admin`

![截圖 2022-09-20 下午8.37.16.png](Day06%20%E6%96%B0%E5%A2%9E%E7%AC%AC%E4%B8%80%E5%80%8B%E7%BE%8E%E5%91%B3%E5%BA%97%E5%AE%B6%20%E5%BB%BA%E7%AB%8B%E8%B3%87%E6%96%99%E5%BA%AB%206fb9f6e050d44f28a5b57aa398bf9208/%25E6%2588%25AA%25E5%259C%2596_2022-09-20_%25E4%25B8%258B%25E5%258D%25888.37.16.png)

就會進入到登入的頁面

![截圖 2022-09-20 下午8.38.28.png](Day06%20%E6%96%B0%E5%A2%9E%E7%AC%AC%E4%B8%80%E5%80%8B%E7%BE%8E%E5%91%B3%E5%BA%97%E5%AE%B6%20%E5%BB%BA%E7%AB%8B%E8%B3%87%E6%96%99%E5%BA%AB%206fb9f6e050d44f28a5b57aa398bf9208/%25E6%2588%25AA%25E5%259C%2596_2022-09-20_%25E4%25B8%258B%25E5%258D%25888.38.28.png)

![截圖 2022-09-20 下午11.07.00.png](Day06%20%E6%96%B0%E5%A2%9E%E7%AC%AC%E4%B8%80%E5%80%8B%E7%BE%8E%E5%91%B3%E5%BA%97%E5%AE%B6%20%E5%BB%BA%E7%AB%8B%E8%B3%87%E6%96%99%E5%BA%AB%206fb9f6e050d44f28a5b57aa398bf9208/%25E6%2588%25AA%25E5%259C%2596_2022-09-20_%25E4%25B8%258B%25E5%258D%258811.07.00.png)

輸入剛剛設定的帳號密碼就可以進入到後台了。

### 手動新增資料

點擊+Add，手動新增我們的第一筆資料，覺丸拉麵藏身於鬧區的巷弄之中，主打濃郁鮮美的豚骨湯頭，除了原味之外也看辣味豚骨、魚介豚骨的選項，從麵條、湯頭、風味油到配料都不馬乎，加之老闆有個性的經營態度，每每到了用餐時段總是一位難求。

![截圖 2022-09-20 下午11.08.54.png](Day06%20%E6%96%B0%E5%A2%9E%E7%AC%AC%E4%B8%80%E5%80%8B%E7%BE%8E%E5%91%B3%E5%BA%97%E5%AE%B6%20%E5%BB%BA%E7%AB%8B%E8%B3%87%E6%96%99%E5%BA%AB%206fb9f6e050d44f28a5b57aa398bf9208/%25E6%2588%25AA%25E5%259C%2596_2022-09-20_%25E4%25B8%258B%25E5%258D%258811.08.54.png)

輸入好之後點擊Save，就可以看到剛剛新增的資料了。

![截圖 2022-09-20 下午11.09.13.png](Day06%20%E6%96%B0%E5%A2%9E%E7%AC%AC%E4%B8%80%E5%80%8B%E7%BE%8E%E5%91%B3%E5%BA%97%E5%AE%B6%20%E5%BB%BA%E7%AB%8B%E8%B3%87%E6%96%99%E5%BA%AB%206fb9f6e050d44f28a5b57aa398bf9208/%25E6%2588%25AA%25E5%259C%2596_2022-09-20_%25E4%25B8%258B%25E5%258D%258811.09.13.png)

### 透過View將資料連結到Template

下一步我們把剛剛新建的Model匯入`view.py`中

```python
from .models import Place
```

接著用`Model.Object.all()`的function將資料傳送到`food/index.html`

```python
def index(request):
    # return HttpResponse("Hello food!")
    return render(request, 'food/index.html', {'store_list': Place.objects.all(), })
```

這邊後面使用字典物件，等一下在html的地方只要呼叫`store_list`就可以取得該Model中的物件

最後我們來到`food/index.html`，找到顯示餐廳頁面的地方

![截圖 2022-09-21 上午11.34.46.png](Day06%20%E6%96%B0%E5%A2%9E%E7%AC%AC%E4%B8%80%E5%80%8B%E7%BE%8E%E5%91%B3%E5%BA%97%E5%AE%B6%20%E5%BB%BA%E7%AB%8B%E8%B3%87%E6%96%99%E5%BA%AB%206fb9f6e050d44f28a5b57aa398bf9208/%25E6%2588%25AA%25E5%259C%2596_2022-09-21_%25E4%25B8%258A%25E5%258D%258811.34.46.png)

原本我們是用編號來暫代餐廳的名稱，現在我們將紅框處改成以下程式碼，從剛剛新建的Model中撈取資料

```html
{% for store in store_list %}
              <div class="col-4 grid-item p-0 m-1">
                <div class="grid-item-img">
                  <img src="https://picsum.photos/id/684/280/200" alt="">
                </div>
                <label for="">{{ store.name }}</label>
              </div>
```

儲存之後就可以run server，進入`/food`看到今天的成果囉～

---

# 🌟本日成果

![截圖 2022-09-21 上午11.49.55.png](Day06%20%E6%96%B0%E5%A2%9E%E7%AC%AC%E4%B8%80%E5%80%8B%E7%BE%8E%E5%91%B3%E5%BA%97%E5%AE%B6%20%E5%BB%BA%E7%AB%8B%E8%B3%87%E6%96%99%E5%BA%AB%206fb9f6e050d44f28a5b57aa398bf9208/%25E6%2588%25AA%25E5%259C%2596_2022-09-21_%25E4%25B8%258A%25E5%258D%258811.49.55.png)

---

# 🛣️ 心得

<aside>
💡 模板語言是個神奇的東西，資料庫也是，版本控制也是TT
筆者在初次嘗試建立資料庫的時候就曾經陰差陽錯下刪除了整個資料庫，有機會的話來講講備份資料庫的故事吧。

</aside>

# 參考資料

[Writing your first Django app, part 2 | Django documentation | Django](https://docs.djangoproject.com/en/4.1/intro/tutorial02/)

[Model field reference | Django documentation | Django](https://docs.djangoproject.com/en/1.10/ref/models/fields/#field-options)

---

> **台南不需要米其林**
> 
> 1. [專案程式碼](https://github.com/yen900611/TNFood_DJ) 
> 2. [專案文件與鐵人賽文章](https://github.com/yen900611/TNFood)
> 3. 參賽團隊 ****[台南巷弄美食獵人](https://ithelp.ithome.com.tw/2022ironman/signup/team/256)****

---