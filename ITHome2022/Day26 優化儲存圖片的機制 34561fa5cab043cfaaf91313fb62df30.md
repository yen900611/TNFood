# Day26  優化儲存圖片的機制

editor: 劭庭
tag: django
進度: 🌕

# 🏁 前言、摘要

<aside>
💡 目前資料庫上傳的圖片大小都不一樣，在顯示的時候除了很凌亂之外，也會影響排版的效果，如果在前端設定圖片大小的話又會被伸縮。我們今天會透過**[django-resized](https://github.com/un1t/django-resized)**這個工具，幫助我們在上傳圖片的時候就設定好圖片的尺寸，搭配限制圖片檔案大小的機制，可以更好的管理圖片。

</aside>

---

# 💡概念說明

**[django-resized](https://github.com/un1t/django-resized)**這項工具除了可以設定圖片的尺寸之外，還可以設定照片裁切的方式，以及儲存圖片的格式。

今天我們預計將所有上傳的圖片都裁切成400 x 400，保留照片的中心並儲存成.png的格式。

---

# 🌟本日成果

![Untitled](Day26%20%E5%84%AA%E5%8C%96%E5%84%B2%E5%AD%98%E5%9C%96%E7%89%87%E7%9A%84%E6%A9%9F%E5%88%B6%2034561fa5cab043cfaaf91313fb62df30/Untitled.png)

首先我們要安裝https://github.com/un1t/django-resized

在Terminal輸入以下指令：

```python
pip install django-resized
```

安裝好之後import到`model.py`中，我們會用到`ResizedImageField`這個類別

```python
from django_resized import ResizedImageField
```

接著我們修改接著我們修改`Photo`的類別

```python
class Photo(models.Model):
    name = models.CharField(max_length=255)
    file = ResizedImageField(size=[400, 400], crop=['middle', 'center'], force_format='PNG', upload_to='photos', validators=[validate_file_size])
    place = models.ForeignKey(Place, help_text="The place that this photo come from.", on_delete=models.SET_NULL,
                              null=True)
```

我們把file那行改成使用`ResizedImageField`

size用來表示圖片的長寬

crop指定裁切的方式，有以下選項

- ['top', 'left']：保留左上角。
- ['middle', 'center']：保留圖片中心。
- ['bottom', 'right']：保留圖片右下角。

force_format則是儲存圖片的格式。

---

# 🛣️ 心得、結語

<aside>
💡 **[django-resized](https://github.com/un1t/django-resized)**雖然沒辦法最精準的切割圖片，但對於只是要簡單限制圖片大小來說還是很夠用了。

</aside>

# 參考資料

1. [https://github.com/un1t/django-resized](https://github.com/un1t/django-resized)

---

> **台南不需要米其林**
> 
> 1. 🏠[專案網址](https://tnfood.pythonanywhere.com/food/)
> 2. 🧑‍💻[專案程式碼](https://github.com/yen900611/TNFood_DJ) 
> 3. 📁[專案文件與鐵人賽文章](https://github.com/yen900611/TNFood)
> 4. 👥參賽團隊 ****[台南巷弄美食獵人](https://ithelp.ithome.com.tw/2022ironman/signup/team/256)****

---