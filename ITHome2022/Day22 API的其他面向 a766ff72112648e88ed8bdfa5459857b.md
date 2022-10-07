# Day22 API的其他面向

editor: 劭庭
tag: django

# 🏁 前言、摘要

<aside>
💡 在前幾天我們透過API建立起前端與後端，但其中還有一些不夠完整的地方，因此今天的目標包括：

- 實作過濾器的API
- 增加API 安全性（POST API ）
</aside>

---

# 🌟本日成果

![Untitled](Day22%20API%E7%9A%84%E5%85%B6%E4%BB%96%E9%9D%A2%E5%90%91%20a766ff72112648e88ed8bdfa5459857b/Untitled.png)

### 實作過濾器的API

改成以API傳遞資料後，收Request中的參數的方式會有點不一樣。Ninja API對於url中自訂的參數都以query的方式解讀，因此我們要在實作API的時候寫入這些參數的名稱。

我們對`places`這個function稍做修改：

```python
@api.get(
    "places",
    response=List[PlacesSchema])
def places(request, food_style:str=None):
```

這邊加入的`food_style`的參數就是我們在`form_filter`中所設定的值，型別是字串，而當使用者還沒送出搜尋時不會收到這項參數，因此我們需要給訂一個預設值。

接著當我們收到參數時，將`food_style`作為篩選值丟入資料庫中撈出對應的店家，如果參數為None則回傳所有店家。

```python
if food_style:
        places = Place.objects.prefetch_related('photo_set', 'tag').filter(tag__style=food_style)
    else:
        places = Place.objects.prefetch_related('photo_set', 'tag').all()
```

在API的頁面上實驗剛剛新增的功能

當url中沒有指定tag時回傳所有店家

![截圖 2022-10-07 下午11.43.15.png](Day22%20API%E7%9A%84%E5%85%B6%E4%BB%96%E9%9D%A2%E5%90%91%20a766ff72112648e88ed8bdfa5459857b/%25E6%2588%25AA%25E5%259C%2596_2022-10-07_%25E4%25B8%258B%25E5%258D%258811.43.15.png)

當指定`food_style`的值時則回傳對應的結果。

![截圖 2022-10-07 下午11.43.15.png](Day22%20API%E7%9A%84%E5%85%B6%E4%BB%96%E9%9D%A2%E5%90%91%20a766ff72112648e88ed8bdfa5459857b/%25E6%2588%25AA%25E5%259C%2596_2022-10-07_%25E4%25B8%258B%25E5%258D%258811.43.15%201.png)

### 增加API 安全性

第二個部分是增加API的安全性，之前我們實作過POST API，這使得他人可以任意往資料庫中新增資料，因此我們需要為API增加權限的限制。

在安全性的部分，有兩種常見的方式，第一種是使用密碼，輸入密碼後才能夠執行API。第二種是JSON Web Token，設定一串英文數字作為token，在有效時間內可以透過Token存取資料。

而今天要實作的是第一種

```python
class BasicAuth(HttpBasicAuth):
    def authenticate(self, request, username, password):
        user = authenticate(username=username, password=password)

@api.post("/tags", auth=BasicAuth())
```

![截圖 2022-10-07 下午10.27.10.png](Day22%20API%E7%9A%84%E5%85%B6%E4%BB%96%E9%9D%A2%E5%90%91%20a766ff72112648e88ed8bdfa5459857b/%25E6%2588%25AA%25E5%259C%2596_2022-10-07_%25E4%25B8%258B%25E5%258D%258810.27.10.png)

這時候如果我們想要新增tag會被阻止

---

# 使用Bear Token

在`api.py`中import`HttpBearer`，並建立一組授權機制：

```python
from ninja.security import HttpBearer

class AuthBearer(HttpBearer):
    def authenticate(self, request, token):
        if token == "supersecret":
            return token
```

將`AuthBearer`加入新增tag的API中：

```python
@api.post("/tags", auth=AuthBearer())
def add_tag(request, tag: Tags):
```

接著我們打開API的介面就可以看到剛剛新增的授權機制囉～

![截圖 2022-10-07 下午10.28.21.png](Day22%20API%E7%9A%84%E5%85%B6%E4%BB%96%E9%9D%A2%E5%90%91%20a766ff72112648e88ed8bdfa5459857b/%25E6%2588%25AA%25E5%259C%2596_2022-10-07_%25E4%25B8%258B%25E5%258D%258810.28.21.png)

輸入token後才能夠執行API

![截圖 2022-10-07 下午10.28.41.png](Day22%20API%E7%9A%84%E5%85%B6%E4%BB%96%E9%9D%A2%E5%90%91%20a766ff72112648e88ed8bdfa5459857b/%25E6%2588%25AA%25E5%259C%2596_2022-10-07_%25E4%25B8%258B%25E5%258D%258810.28.41.png)

# 參考資料

1. [Authentication - Django Ninja (rest-framework.com)](https://django-ninja.rest-framework.com/guides/authentication/)
2. [Query parameters - Django Ninja (rest-framework.com)](https://django-ninja.rest-framework.com/guides/input/query-params/)

---

> **台南不需要米其林**
> 
> 1. 🏠[專案網址](https://tnfood.pythonanywhere.com/food/)
> 2. 🧑‍💻[專案程式碼](https://github.com/yen900611/TNFood_DJ) 
> 3. 📁[專案文件與鐵人賽文章](https://github.com/yen900611/TNFood)
> 4. 👥參賽團隊 ****[台南巷弄美食獵人](https://ithelp.ithome.com.tw/2022ironman/signup/team/256)****

---