# Day14 不同類型的美味

editor: 劭庭

# 🏁 前言、摘要

<aside>
💡 在第二階段的目標是要透過餐廳的類型及設備篩選出心目中的餐廳。因此我們需要先建立類型（Tags）跟設備（Device）的資料庫。這需要用到Django裡面多對多的資料庫連結方式（`ManyToManyField`）。

</aside>

---

# 💡概念說明

![Untitled](Day14%20%E4%B8%8D%E5%90%8C%E9%A1%9E%E5%9E%8B%E7%9A%84%E7%BE%8E%E5%91%B3%20a07e1419eee04d608a428a93e6b1f1d5/Untitled.png)

---

# 🌟本日成果

### 建立Tag資料庫

在這個專案中我們會依據店家提供的食物類型為店家增加不同的「tag」，一家店可以有兩個以上的tag，同一個tag當然也會被用在不同的店家之中。

目前預計會新增的tag如下：

- Tag
    - 台式
    - 韓式
    - 中式
    - 日式
    - 美式
    - 義式
    - 德式
    - 英式
    - 港式
    - 越式
    - 泰式
    - 全素
    - 蛋素
    - 奶素
    - 五辛素
    - 海鮮
    - 甜點
    - 咖啡
    - 茶
    - 酒吧
    - 火鍋
    - 燒烤
    - 拉麵
    - 飯
    - 麵
    - 粥
    - 餃
    - 冰品
    - 甜湯
    - 合菜
    - 簡餐

打開`model.py`，我們同樣根據之前的方式新增`Tag`的資料庫

```python
class Tag(models.Model):
    name = models.CharField(max_length=10)
		Rstyle = models.CharField(max_length=30, null=True)

    def __str__(self):
        return self.name
```

接下來我們回到Place，在原有的程式碼中增加與Tag資料庫的關聯（新增最後一行）

```python
class Place(models.Model):
    name = models.CharField(max_length=20, help_text='Enter the name of store')
    address = models.CharField(max_length=50, null=True)
    phone_number = models.CharField(max_length=20, default="No phone number")
    web_site = models.CharField(max_length=200, default="No web site")
    introduction = models.CharField(max_length=100, default="不用問去吃就對了")
    pub_date = models.DateTimeField('date published')
    tags = models.ManyToManyField(Tag, through='Tag_Management', through_fields=('place', 'tags'))

def __str__(self):
        return self.name
```

這邊我們把新增的程式碼拉除來單獨仔細說明。

```python
tags = models.ManyToManyField(Tag, through='Tag_Management', through_fields=('place', 'tags'))
```

我們使用`models.ManyToManyField`來表示這兩個資料庫是多對多的關係（一種Tag會用在很多Place，一個Place也會有多個Tag），而在Django中對於多對多的資料，需要一個第三方的資料庫來儲存，也就是`through`，在這個欄位我們要指定一個新的資料庫名稱。

### 建立Tag_Management資料庫

作為兩個資料庫的中介，新建立的資料庫名稱必須與上面`through`欄位完全相同。可以把這個資料庫想像成是儲存另外兩個資料庫的關聯關係，因此每筆資料中會包含一個`Tag`跟一個`Place`。

```python
class Tag_Management(models.Model):
    place = models.ForeignKey(Place, on_delete=models.SET_NULL)
    tags = models.ForeignKey(Tag, on_delete=models.SET_NULL)
```

### 新增admin欄位並更新資料庫

每次新增資料庫後都要記得在`admin.py`中增加欄位，包括`Tag_management`也是

```python
admin.site.register(Tag, admin.ModelAdmin)
admin.site.register(Tag_Management, admin.ModelAdmin)
```

接著更新資料庫

```python
python manage.py makemigrations
python manage.py migrate
```

run server後我們就可以看到剛剛新增的欄位了

![tag_admin.png](Day14%20%E4%B8%8D%E5%90%8C%E9%A1%9E%E5%9E%8B%E7%9A%84%E7%BE%8E%E5%91%B3%20a07e1419eee04d608a428a93e6b1f1d5/tag_admin.png)

這邊我們新增一個「拉麵」的Tag，再進入Tag_managements將「覺丸拉麵」與「拉麵」連結在一起

![tag_management.png](Day14%20%E4%B8%8D%E5%90%8C%E9%A1%9E%E5%9E%8B%E7%9A%84%E7%BE%8E%E5%91%B3%20a07e1419eee04d608a428a93e6b1f1d5/tag_management.png)

### 建立Device資料庫

接著我們用一樣的方法新增Deveice資料庫，用來儲存店家的設備資訊，例如有沒有冷氣（這在台南絕對是超級重要）、是否可以刷卡、是否提供WIFI等等，同樣也是多對多的資料庫。

### `Model.py`

```python
class Device(models.Model):
    name = models.CharField(max_length=20)

    def __str__(self):
        return self.name
```

```python
class Device_Management(models.Model):
    place = models.ForeignKey(Place, on_delete=models.CASCADE)
    devices = models.ForeignKey(Device, on_delete=models.CASCADE)
```

### `admin.py`

```python
admin.site.register(Device, admin.ModelAdmin)
admin.site.register(Device_Management, admin.ModelAdmin)
```

![device.png](Day14%20%E4%B8%8D%E5%90%8C%E9%A1%9E%E5%9E%8B%E7%9A%84%E7%BE%8E%E5%91%B3%20a07e1419eee04d608a428a93e6b1f1d5/device.png)

### 新增資料

現在我們有多欄位可以填寫了！今天我們先增加最重要的素別，來介紹一些素食店家～

![素食類別.png](Day14%20%E4%B8%8D%E5%90%8C%E9%A1%9E%E5%9E%8B%E7%9A%84%E7%BE%8E%E5%91%B3%20a07e1419eee04d608a428a93e6b1f1d5/%25E7%25B4%25A0%25E9%25A3%259F%25E9%25A1%259E%25E5%2588%25A5.png)

小鳥不吃肉是今年新開在東區的素食店，形式上類似於便當，味道卻遠勝於一般的便當店（包括葷食！）主菜有豐富的選擇，包括菇類、素肉類及豆製品類，即使吃不慣素肉也可以試試看麻婆豆腐跟趙燒豆皮！三格小菜店家也相當用心處理，清爽的料理方式有別於一般便當店的油膩，也不會只有小白菜青江菜等普通葉菜類，好吃程度讓人忘記自己是在吃素食！

![小鳥不吃肉.png](Day14%20%E4%B8%8D%E5%90%8C%E9%A1%9E%E5%9E%8B%E7%9A%84%E7%BE%8E%E5%91%B3%20a07e1419eee04d608a428a93e6b1f1d5/%25E5%25B0%258F%25E9%25B3%25A5%25E4%25B8%258D%25E5%2590%2583%25E8%2582%2589.png)

全店除了貢丸湯是蛋素之外，其餘料理皆是全素。

![全素.png](Day14%20%E4%B8%8D%E5%90%8C%E9%A1%9E%E5%9E%8B%E7%9A%84%E7%BE%8E%E5%91%B3%20a07e1419eee04d608a428a93e6b1f1d5/%25E5%2585%25A8%25E7%25B4%25A0.png)

![蛋素.png](Day14%20%E4%B8%8D%E5%90%8C%E9%A1%9E%E5%9E%8B%E7%9A%84%E7%BE%8E%E5%91%B3%20a07e1419eee04d608a428a93e6b1f1d5/%25E8%259B%258B%25E7%25B4%25A0.png)

---

![截圖 2022-09-29 下午9.12.02.png](Day14%20%E4%B8%8D%E5%90%8C%E9%A1%9E%E5%9E%8B%E7%9A%84%E7%BE%8E%E5%91%B3%20a07e1419eee04d608a428a93e6b1f1d5/%25E6%2588%25AA%25E5%259C%2596_2022-09-29_%25E4%25B8%258B%25E5%258D%25889.12.02.png)

# 🛣️ 心得、結語

<aside>
💡 店家越來越多啦～明天會實作filter的部分，希望資料庫的地方對大家來說不會太複雜

</aside>

# 參考資料

1. 

[Django ORM 模型層 ManyToMany(多對多)](https://medium.com/my-back-end-life/django-orm-%E6%A8%A1%E5%9E%8B%E5%B1%A4-manytomany-%E5%A4%9A%E5%B0%8D%E5%A4%9A-34dc9faef65c)

[The right way to use a ManyToManyField in Django](https://www.sankalpjonna.com/learn-django/the-right-way-to-use-a-manytomanyfield-in-django)

---

> **台南不需要米其林**
> 
> 1. 🏠[專案網址](https://tnfood.pythonanywhere.com/food/)
> 2. 🧑‍💻[專案程式碼](https://github.com/yen900611/TNFood_DJ) 
> 3. 📁[專案文件與鐵人賽文章](https://github.com/yen900611/TNFood)
> 4. 👥參賽團隊 ****[台南巷弄美食獵人](https://ithelp.ithome.com.tw/2022ironman/signup/team/256)****

---