# Day17 保存好味道 談資料管理與備份

editor: 麒麟
tag: Django, admin, 資料備份
進度: 🌕

# 🏁 摘要

<aside>
💡 一個網站系統，最重要的就是系統儲存的資料，我們在本地有開發用的資料庫，`PythonAnywhere`上面有一個SQLite的資料庫，隨著我們新增愈多的商家，就愈需要做好資料的備份。
今天我們會說明資料備份移轉的方式，並且使用Django的一些套件，來管理我們的系統。

</aside>

---

# 💡概念說明

1. 資料庫備份與移轉
    - 資料庫系統可以設定備份，通常會有兩三個資料庫系統互做備援。
    - 如果要跨系統的移轉資料，基本上需要將Ａ資料庫的資料匯出成檔案，再匯入Ｂ資料庫
    - 在Django之中，我們可以將資料庫的資料匯出成檔案，也可以檔案重新匯入資料庫。
    - 可參考
    [https://docs.djangoproject.com/en/3.2/ref/django-admin/#dumpdata](https://docs.djangoproject.com/en/3.2/ref/django-admin/#dumpdata)
2. Django 管理介面
    - Django 有完整的後台系統，可以使用一個****ModelAdmin****的Class，來設定顯示我們需要的資料。
    - [https://docs.djangoproject.com/en/3.2/ref/contrib/admin/](https://docs.djangoproject.com/en/3.2/ref/contrib/admin/)

---

# 🌟本日成果

![Untitled](Day17%20%E4%BF%9D%E5%AD%98%E5%A5%BD%E5%91%B3%E9%81%93%20%E8%AB%87%E8%B3%87%E6%96%99%E7%AE%A1%E7%90%86%E8%88%87%E5%82%99%E4%BB%BD%20ccd0203a866549998bc596a31cd2757b/Untitled.png)

## 資料庫備份

1. 匯出資料
    - dumpdata 指令可以將資料庫的資料會出成檔案，預設為JSON格式的字串。
    
    ```bash
    python manage.py dumpdata --natural-foreign --natural-primary \
    -e contenttypes -e auth -e admin -e sessions \
    --indent 2 -o ./db/latest_db.json
    ```
    
    - `--natural-foreign` `--natural-primary` 這兩個參數跟資料的 `PK(primary key)` `FK(foreign key)`有關。
    先說結論：加上這兩個參數，會增加匯出檔案的大小，但是可以解決資料衝突的問題。
        - `PK` 是資料在資料庫中獨一無二的編號，可能是整數或是`UUID`，就是流水號的概念。
        - 在Ａ資料表中，使用Ｂ資料表的`PK`，就稱作`FK`
            - 我們在`Tag_Management` 這個表中有兩個`FK`分別是`Tag` 和 `Place`，表示某一間店有某一個標籤。
            
            ```python
            # food/models.py
            class Tag_Management(models.Model):
                **place** = models.**ForeignKey**(**Place**, on_delete=models.SET_NULL, null=True)
                **tags** = models.**ForeignKey**(**Tag**, on_delete=models.SET_NULL, null=True)
            ```
            
        - 假設資料庫Ａ匯出的店家有一個`pk=1` 名叫覺丸拉麵，但是資料庫Ｂ已經有一個`pk=1`的小鳥不吃肉，這樣直接匯入資料的話，對應就會出錯，因此加上這個參數可以自動處理這個問題。
    - `-e` or `--exclude` 這是可以去除某些資料，像是session  contenttypes，這種資料是暫時的不是很重要去除掉會比較好。auth admin 則是跟使用者資料有關，這是相對敏感的資料，所以在備份時需要注意。
2. 匯入資料
    
    匯入資料就相對簡單，直接指定檔案的路徑即可。
    
    ```bash
    python manage.py loaddata ./db/latest_db.json
    ```
    
3. 實際使用方式
    - 筆者會將開發過程新增的照片、新增的店家資料匯出，並且加入git之中，方便我們上傳至`PythonAnywhere`。
    - 如果有自己的伺服器，比較好的做法是用 `scp` 指令將檔案上傳到伺服器中，再匯入資料，一個是減少git的檔案，一個是避免敏感資料外洩。
    - 如果是在`PythonAnywhere`匯出資料，可以直接透過網頁下載。
        
        ![django_files.png](Day17%20%E4%BF%9D%E5%AD%98%E5%A5%BD%E5%91%B3%E9%81%93%20%E8%AB%87%E8%B3%87%E6%96%99%E7%AE%A1%E7%90%86%E8%88%87%E5%82%99%E4%BB%BD%20ccd0203a866549998bc596a31cd2757b/django_files.png)
        

## Django 管理介面

1. 直接在`food/admin.py` 加入下方程式碼
    
    ```python
    # food/admin.py 
    class PlaceAdmin(admin.ModelAdmin):
        list_display = ('id', 'name', 'address', 'web_site', 'pub_date')
        list_filter = ( 'pub_date','tags')
        search_fields = ('name', 'address', 'web_site')
        ordering = ('-id',)
    ```
    
    ![place_adminsite.png](Day17%20%E4%BF%9D%E5%AD%98%E5%A5%BD%E5%91%B3%E9%81%93%20%E8%AB%87%E8%B3%87%E6%96%99%E7%AE%A1%E7%90%86%E8%88%87%E5%82%99%E4%BB%BD%20ccd0203a866549998bc596a31cd2757b/place_adminsite.png)
    
    - 各欄位對應的介面如上圖，`ordering`使用`-` 就表示將遞減排序。
    - 此[文章](http://dokelung-blog.logdown.com/posts/220832-django-notes-6-manage-your-system-admin)有詳細的說明，官網還有更多的資訊。
2. 編輯`Place`時，可以看到`Tag`
    - 使用 `inlines`，可以將`Tag`的介面嵌入在下方。
        
        ```python
        class TagInlineAdmin(admin.StackedInline):
            model = Place.tags.through
        
        class PlaceAdmin(admin.ModelAdmin):
            list_display = ('id', 'name', 'address', 'web_site', 'pub_date')
            list_filter = ('pub_date', 'tags')
            search_fields = ('name', 'address', 'web_site')
            ordering = ('-id',)
            readonly_fields = ('pub_date',)
            inlines = [TagInlineAdmin, ]
        ```
        
    - 結果
        
        ![place_adminsite_single.png](Day17%20%E4%BF%9D%E5%AD%98%E5%A5%BD%E5%91%B3%E9%81%93%20%E8%AB%87%E8%B3%87%E6%96%99%E7%AE%A1%E7%90%86%E8%88%87%E5%82%99%E4%BB%BD%20ccd0203a866549998bc596a31cd2757b/place_adminsite_single.png)
        

---

# 🛣️ 心得

<aside>
💡 完成了這個章節，更加感受到Django的方便與強大，這樣一來其實我們不用實作後台介面，就可以增減更新資料。

</aside>

# 參考資料

1. Django Admin
    - [http://dokelung-blog.logdown.com/posts/220832-django-notes-6-manage-your-system-admin](http://dokelung-blog.logdown.com/posts/220832-django-notes-6-manage-your-system-admin)

---

> **台南不需要米其林**
> 
> 1. 🏠[專案網址](https://tnfood.pythonanywhere.com/food/)
> 2. 🧑‍💻[專案程式碼](https://github.com/yen900611/TNFood_DJ) 
> 3. 📁[專案文件與鐵人賽文章](https://github.com/yen900611/TNFood)
> 4. 👥參賽團隊 ****[台南巷弄美食獵人](https://ithelp.ithome.com.tw/2022ironman/signup/team/256)****

---