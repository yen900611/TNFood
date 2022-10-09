# Day24 管理美食資料庫的細節

editor: 劭庭

# 🏁 前言、摘要

<aside>
💡 隨著網站內容逐漸增加，會需要解決的問題也隨之而來，例如上傳了過多的照片導致儲存空間不足，以及使用人數的掌握。因此我們今天的目標有兩個，一個是限制圖片上傳的檔案大小，一個是實作統計網站瀏覽人數的API。

</aside>

- [x]  限制圖片上傳大小
- [x]  提供使用者人數的API
    
    ```json
    {
    "today":10,
    "total":1000
    }
    ```
    

---

# 💡概念說明

在限制圖片大小的部分，我們會透過validate的功能，這是django中用來限制資料的驗證器，他可以設下限制，並且跳出錯誤訊息提醒使用者。

至於統計使用者人數，會使用到session，這是Django對於每個造訪網站的裝置自動保存的資料，因此我們只要統計session的個數，就能大落掌握目前網的使用情況。

理論到位，實作開始！

---

# 🌟本日成果

![Untitled](Day24%20%E7%AE%A1%E7%90%86%E7%BE%8E%E9%A3%9F%E8%B3%87%E6%96%99%E5%BA%AB%E7%9A%84%E7%B4%B0%E7%AF%80%20caa6124403ae499eab02d352d8b38cc3/Untitled.png)

### 限制圖片上傳大小

我們進入我們進入`model.py`，在`Photo`的上方新增下列程式碼：

```python
def validate_file_size(value):
    filesize = value.size
    if filesize > 10240:
        raise ValidationError("The maximum file size that can be uploaded is 10KB")
    else:
        return value

```

這裡我們先抓出檔案的大小，並限制如果超過10KB，就顯示`ValidationError`

並將上傳檔案那行改成這樣：

```python

class Photo(models.Model):
    name = models.CharField(max_length=255)
    file = models.ImageField(upload_to='photos', validators=[validate_file_size])
    place = models.ForeignKey(Place, help_text="The place that this photo come from.", on_delete=models.SET_NULL,
                              null=True)
```

來看看成果

![截圖 2022-10-09 下午8.51.41.png](Day24%20%E7%AE%A1%E7%90%86%E7%BE%8E%E9%A3%9F%E8%B3%87%E6%96%99%E5%BA%AB%E7%9A%84%E7%B4%B0%E7%AF%80%20caa6124403ae499eab02d352d8b38cc3/%25E6%2588%25AA%25E5%259C%2596_2022-10-09_%25E4%25B8%258B%25E5%258D%25888.51.41.png)

### 建立使用者人數的API

首先我們建立一個使用人數的Schema

```python
class Visit_num(Schema):
    today: int
    total: int
```

接著建立一個`GET`的API

```python
@api.get("num_visits", response=Visit_num)
def visit_number(request):
    num_visits = request.session.get('num_visits', 0)
    request.session['num_visits'] = num_visits + 1
    return Visit_num(today = num_visits, total = num_visits)
```

![截圖 2022-10-09 下午9.06.18.png](Day24%20%E7%AE%A1%E7%90%86%E7%BE%8E%E9%A3%9F%E8%B3%87%E6%96%99%E5%BA%AB%E7%9A%84%E7%B4%B0%E7%AF%80%20caa6124403ae499eab02d352d8b38cc3/%25E6%2588%25AA%25E5%259C%2596_2022-10-09_%25E4%25B8%258B%25E5%258D%25889.06.18.png)

---

# 🛣️ 心得、結語

<aside>
💡 明明才放沒幾間店就被通知雲端容量要爆炸了QAQ嚇得筆者趕快幫自己建立一個限制器。

</aside>

# 參考資料

1. [Validators | Django documentation | Django (djangoproject.com)](https://docs.djangoproject.com/en/4.1/ref/validators/)
2. [Django Tutorial Part 7: Sessions framework - Learn web development | MDN (mozilla.org)](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Sessions)

---

> **台南不需要米其林**
> 
> 1. 🏠[專案網址](https://tnfood.pythonanywhere.com/food/)
> 2. 🧑‍💻[專案程式碼](https://github.com/yen900611/TNFood_DJ) 
> 3. 📁[專案文件與鐵人賽文章](https://github.com/yen900611/TNFood)
> 4. 👥參賽團隊 ****[台南巷弄美食獵人](https://ithelp.ithome.com.tw/2022ironman/signup/team/256)****

---