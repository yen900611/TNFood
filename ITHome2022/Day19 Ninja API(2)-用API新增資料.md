# Day19 Ninja API(2)-用API新增資料

editor: 劭庭
tag: django, ninja, python

# 🏁 前言、摘要

<aside>
💡 昨天我們嘗試用Ninja API抓到資料庫中的資料，不過API的功能不只可以從資料庫中撈出資料，也可以反向寫入資料，今天我們就來嘗試這部分的功能。

</aside>

---

# 💡概念說明

![Untitled](Day19%20Ninja%20API(2)-%E7%94%A8API%E6%96%B0%E5%A2%9E%E8%B3%87%E6%96%99%2088dfa63a96a046f48fce40b07246afe6/Untitled.png)

## HTTP Request的類型

在今天的實作之前，我們先完整的說明HTTP Request的各種類型，不同類型的Request會需要實作不同的API。

首先，什麼是Request呢？簡單理解成使用者對網站開出的請求，例如送出一個網址，希望看到網站的內容，或是想要編輯網頁的內容，不同的請求就會對應到九種不同類型的Request：

- GET：第一種也是我們目前為止唯一提過的一種。像網頁索取指定資料。
- HEAD：與GET相同，但不會收到整個傳送內容(`Body`)，只會收到標頭檔。
- POST：將資料傳送給網頁，例如上傳檔案、送出表單等等。
- OPTION：向後端索取針對特定資源可以使用的指令。
- PUT：在原有的指定位置上更新內容。
- DELETE：刪除URL中特定資料。
- CONNECT：Http1.1中可以與特定資料庫連接的管道。
- TRACE：回傳網站收到的Request，通常用於測試。
- PATCH：對資料進行部分修改。

而我們今天要實作的是POST。

---

# 🌟本日成果

今天我們將實作新增Tag資料的POST Request。

進入`api.py`，新增以下程式碼：

```python
@api.post("tags")
def add_tag(request, pay_load:Tags):
    tag = Tag.objects.create(**pay_load.dict())
    return {"id":tag.id}
```

第一行的`api.post`代表這個function是要處理POST。

`payload`在這邊我們丟入昨天實作過的Tag的`Schema`，並在第二行中指定成輸入的格式。

`Tag.objects.create`則是透過程式碼對資料庫新增資料的方式。

![截圖 2022-10-04 下午11.04.13.png](Day19%20Ninja%20API(2)-%E7%94%A8API%E6%96%B0%E5%A2%9E%E8%B3%87%E6%96%99%2088dfa63a96a046f48fce40b07246afe6/%25E6%2588%25AA%25E5%259C%2596_2022-10-04_%25E4%25B8%258B%25E5%258D%258811.04.13.png)

進入Ninja之後我們一樣試用今天新增的API（注意！在這邊新增的資料都不是真正的編輯資料庫喔！）

可以看到在Request body的部分，出現了字典的格式，這是因為我們剛剛在程式中設定的緣故。

輸入好之後就可以點擊Execute，就可以看到網頁的Responce囉～

![截圖 2022-10-04 下午11.06.28.png](Day19%20Ninja%20API(2)-%E7%94%A8API%E6%96%B0%E5%A2%9E%E8%B3%87%E6%96%99%2088dfa63a96a046f48fce40b07246afe6/%25E6%2588%25AA%25E5%259C%2596_2022-10-04_%25E4%25B8%258B%25E5%258D%258811.06.28.png)

---

# 參考資料

1. [HTTP request methods - HTTP | MDN (mozilla.org)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)
2. [CRUD example - Django Ninja (rest-framework.com)](https://django-ninja.rest-framework.com/tutorial/other/crud/)

---

> **台南不需要米其林**
> 
> 1. 🏠[專案網址](https://tnfood.pythonanywhere.com/food/)
> 2. 🧑‍💻[專案程式碼](https://github.com/yen900611/TNFood_DJ) 
> 3. 📁[專案文件與鐵人賽文章](https://github.com/yen900611/TNFood)
> 4. 👥參賽團隊 ****[台南巷弄美食獵人](https://ithelp.ithome.com.tw/2022ironman/signup/team/256)****

---