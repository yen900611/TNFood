# Day18 前後端分離的第一步－Ninja API

editor: 劭庭
tag: django, ninja, python

# 🏁 前言、摘要

<aside>
💡 今天我們會透過Ninja API這個工具，來了解網站中的API的功能，為什麼需要使用API？要怎麼實作？一起看下去吧～

</aside>

---

# 💡概念說明

![Untitled](Day18%20%E5%89%8D%E5%BE%8C%E7%AB%AF%E5%88%86%E9%9B%A2%E7%9A%84%E7%AC%AC%E4%B8%80%E6%AD%A5%EF%BC%8DNinja%20API%20f8e4173349aa4686a633c4f62c19cc26/Untitled.png)

# API 概念 為何需要使用API

API的全名是Application Programming Interface，在不同的地方會呈現不同的形式。而在網站上，API負責前端與後端的資料傳輸，如果我們將網站比喻成一個餐廳，使用者在網站上送出請求就像是顧客點餐，廚房內部（前端）收到訂單（Request）後，就要根據訂單的內容跟食材庫（後端、DB）拿食材（資料），而API就是負責到食材庫裡面將材料送到廚房手中的角色。

使用API可以讓我們將網站的前端與後端分開處理、測試，除此之外也可以讓拿資料的方式變得更方便。延續前面餐廳的例子，當前後端與食材庫沒有分離，廚房中每次需要同一種食材時，都要個別進入食材庫，但我們可以透過API管理不同的食材，這樣每次只要對特定的API就可以拿到該API負責管理的資料。

## 使用方式：API模板

首先我們需要安裝Ninja，到Terminal輸入以下指令：

```bash
pip install django-ninja
```

我們根據前端的需求，先建立一個可以讓我們拿到所有tag資料的api，我們在`/mysite/food`的資料夾底下新增`api.py`，並寫入以下程式碼：

```python
from typing import List
from ninja import NinjaAPI, Schema
from .models import Tag

api = NinjaAPI()

class Tags(Schema):
    name: str
    style: str

@api.get("tags", response=List[Tags])
def tags(request):
    tag = Tag.objects.all()
    return tag
```

這邊我們詳細介紹一下程式碼：

`Schema`是一個可以幫助我們檢查資料格式的類別，包含欄位的格式、資料型別等等。

首先我們建立一個NinjaAPI，並定義前端要取的的資料格式。

`@api.get`代表這個request是GET，而response則是定義回傳內容的格式，這邊預計用list的形式回傳。

接著我們到`/mysite/mysite/url.py`檔案裡面，新增以下程式碼：

```python
from food.api import api
urlpatterns = \
    [
        path('food-admin/', admin.site.urls),
        path('food/', include('food.urls')),
        path("api/", api.urls),
        path('', index)
    ] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

這邊是為了將api的頁面加入路徑。

最後我們runserver，在網址輸入：

```python
http://127.0.0.1:8000/api/docs
```

![截圖 2022-10-03 下午10.57.59.png](Day18%20%E5%89%8D%E5%BE%8C%E7%AB%AF%E5%88%86%E9%9B%A2%E7%9A%84%E7%AC%AC%E4%B8%80%E6%AD%A5%EF%BC%8DNinja%20API%20f8e4173349aa4686a633c4f62c19cc26/%25E6%2588%25AA%25E5%259C%2596_2022-10-03_%25E4%25B8%258B%25E5%258D%258810.57.59.png)

點擊「Try it out」然後執行，就可以看到API到後端撈出的Tag資料了

![截圖 2022-10-03 下午10.59.41.png](Day18%20%E5%89%8D%E5%BE%8C%E7%AB%AF%E5%88%86%E9%9B%A2%E7%9A%84%E7%AC%AC%E4%B8%80%E6%AD%A5%EF%BC%8DNinja%20API%20f8e4173349aa4686a633c4f62c19cc26/%25E6%2588%25AA%25E5%259C%2596_2022-10-03_%25E4%25B8%258B%25E5%258D%258810.59.41.png)

Ninja API中還有許多今天沒有用到的功能，大家可以輸入下面的範例程式碼自己玩玩看喔～

```python
class TemplateResponseSchema(Schema):
    param1: bool = True
    param2: int = 1
    param3: str = "hello api template"
    param4: List = []
    param5: Dict = {}
    request_data: str = "request"

@template_router.post(
    "template/{url_param1}",
    summary="summary to template a api function",
    # description="short description",
    response=TemplateResponseSchema,
    tags=['template'],
    auth=None)
def api_template(request, url_param1: str, body: TemplateRequestSchema):
"""
        # Long markdown api H1 title
        ## H2
         - bullet list
         1. number list
    """
# collect data and do something

    return TemplateResponseSchema(request_data=str(body))
```

![api_template.png](Day18%20%E5%89%8D%E5%BE%8C%E7%AB%AF%E5%88%86%E9%9B%A2%E7%9A%84%E7%AC%AC%E4%B8%80%E6%AD%A5%EF%BC%8DNinja%20API%20f8e4173349aa4686a633c4f62c19cc26/api_template.png)

---

# 🌟本日成果

1. 設計API
    - 取得所有分類標籤的清單
    - 取得所有的店家資訊的清單
        - 一個店家的資訊要有店名、地址、電話、一張照片
    - 取得單一個店家的基本資料、標籤、照片
2. 實作
3. 回傳結果
    
    ![截圖 2022-10-03 下午10.13.19.png](Day18%20%E5%89%8D%E5%BE%8C%E7%AB%AF%E5%88%86%E9%9B%A2%E7%9A%84%E7%AC%AC%E4%B8%80%E6%AD%A5%EF%BC%8DNinja%20API%20f8e4173349aa4686a633c4f62c19cc26/%25E6%2588%25AA%25E5%259C%2596_2022-10-03_%25E4%25B8%258B%25E5%258D%258810.13.19.png)
    

---

# 參考資料

1. 

---

> **台南不需要米其林**
> 
> 1. 🏠[專案網址](https://tnfood.pythonanywhere.com/food/)
> 2. 🧑‍💻[專案程式碼](https://github.com/yen900611/TNFood_DJ) 
> 3. 📁[專案文件與鐵人賽文章](https://github.com/yen900611/TNFood)
> 4. 👥參賽團隊 ****[台南巷弄美食獵人](https://ithelp.ithome.com.tw/2022ironman/signup/team/256)****

---