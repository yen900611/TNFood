# Day12 系統架構設計與安全性維護

進度: 🌕

# 🏁 前言

<aside>
💡 第二階段的系統需求和介面已經完成，過程中有發現一些問題，因此除了新的需求需要實作以外，我們也需要調整一下系統架構。
前兩天剛部署上去的服務，有一些安全性問題，在第二階段開始實作前，也需要先處理一下。

</aside>

---

# 🌟本日成果

## 系統架構設計

![Untitled](Day12%20%E7%B3%BB%E7%B5%B1%E6%9E%B6%E6%A7%8B%E8%A8%AD%E8%A8%88%E8%88%87%E5%AE%89%E5%85%A8%E6%80%A7%E7%B6%AD%E8%AD%B7%208f3b747ecba543beaa79a7a4792f8ed8/Untitled.png)

- 在開發的過程發現，只要後端的資料一改動，變數名稱不對，模板程式就無法正確顯示資料，短期來說不至於會有問題，但隨著專案變大，就會花愈多的時間確認後端資料與前端畫面的對應關係。
- 因此現在很多服務都走向了前後端分離，讓兩邊可以單獨運作分工開發，這次我們也會將系統修正為前後端分離的形式。

## 系統架構圖 v2

第二個階段，我們將往前後端分離邁進，`Djano`本身就有辦法設計後端的API，也有辦法提供前端頁面，在這個階段我們先都使用Django內建的功能做到下列圖示的結果。前端有單純的`HTML` `CSS` `JS`程式碼，並且透過API跟 後端要資料，並顯示在畫面上。

![Untitled](IT%E9%90%B5%E4%BA%BA%E8%B3%BD%20ac998c66ed7d4f36b2bfea77821f3668/%E5%B0%88%E6%A1%88%E6%96%87%E4%BB%B6%2062ccf9cd84a3418ab9c39636cc7110f8/Project%20Doc%202c811c7ee4b84fd1a1e439db431de290/Untitled.png)

> 箭頭表示，使用者可以存取到前端的頁面。
而前端頁面，可以透過後端API抓到資料。
> 

## 資料架構圖 v2

我們增加了過濾器的功能，因此新增一個標籤的資料類型(Tag)，用來表示各式各樣的標籤，而一家店會有多種的標籤

（像是台南小巷中的`[小鳥不吃肉](https://www.facebook.com/VeggieBirds/)`同時就會有`全素`、`中式`、`內用`、`冷氣`等標籤）。因此店家與標籤的關係是多對多的關係，因此可以畫成下方的圖表。

![Untitled](IT%E9%90%B5%E4%BA%BA%E8%B3%BD%20ac998c66ed7d4f36b2bfea77821f3668/%E5%B0%88%E6%A1%88%E6%96%87%E4%BB%B6%2062ccf9cd84a3418ab9c39636cc7110f8/Project%20Doc%202c811c7ee4b84fd1a1e439db431de290/Untitled%201.png)

## 安全性維護

![Untitled](Day12%20%E7%B3%BB%E7%B5%B1%E6%9E%B6%E6%A7%8B%E8%A8%AD%E8%A8%88%E8%88%87%E5%AE%89%E5%85%A8%E6%80%A7%E7%B6%AD%E8%AD%B7%208f3b747ecba543beaa79a7a4792f8ed8/Untitled%201.png)

[https://docs.djangoproject.com/en/3.2/howto/deployment/checklist/](https://docs.djangoproject.com/en/3.2/howto/deployment/checklist/)

- `SECRET_KEY` 是Django用來加密的，如果被他人知道，就有辦法解密瀏覽器與伺服器之間溝通的資訊，造成安全性疑慮。一般來說當然不會被知道，但是像是我們將程式碼上傳至github這種公開的平台，就相當不妥，因此也需要重新設定一下。
- 關閉Debug模式
    
    部署在[這裡](https://tnfood.pythonanywhere.com/food/)的網站，如果亂按的話，會出現Django預設的錯誤資訊，這其實是很危險的，因為駭客能因此知道運作的環境與伺服器的詳細資料。
    需要把 DEBUG 改成 False才行
    
    ```python
    # mysite/settings.py
    DEBUG = False
    ```
    
- admin 路徑
    
    Django 自帶後台功能，相當的方便，不過只要能輸入帳號密碼的地方，就有安全的疑慮，我們可以用強力的帳號密碼來防止駭客入侵，但是能不被攻擊總是好的。因此我們可以更改預設的管理入口，減少被攻擊的機會。
    
    ```python
    # mysite/urls.py
    urlpatterns = \
        [
            path('my-custom-admin/', admin.site.urls),
            path('food/', include('food.urls')),
            path('', index)
        ] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
    ```
    
- 因為我們會將程式碼上傳至公開平台，因此不能在`settings.py`放入敏感資訊，這時我們可以使用環境變數來管理開發環境和生產環境，不同的設定檔。以下用`DEBUG` 參數為例
    1. 建立`.env`
        
        ```
        DEBUG='False'
        ```
        
    2. 安裝 `python-dotenv`
        
        ```bash
        pip install python-dotenv
        ```
        
    3. 在settings.py中匯入 .env
        
        ```python
        from dotenv import load_dotenv
        load_dotenv(Path.joinpath(BASE_DIR, '.env'))  # take environment variables from .env.
        if os.getenv('DEBUG') == 'False':
            # SECURITY WARNING: don't run with debug turned on in production!
            DEBUG = False
        else:
            DEBUG = True
        ```
        
    4. 記得`.env` 是不能列入git管理的喔
    5. 開發環境中和生產環境的.env內容可能會不同，因此也可以放入敏感的資料庫帳號密碼等。

---

# 🛣️ 心得、結語

<aside>
💡 接下來就要開始第二階段啦，請大家敬請期待。

</aside>

# 參考資料

1. ****[原來後端要知道] 什麼是前後端分離？**** [https://ithelp.ithome.com.tw/articles/10231011](https://ithelp.ithome.com.tw/articles/10231011)
2. 部署注意事項
    
    [https://docs.djangoproject.com/en/3.2/howto/deployment/checklist/](https://docs.djangoproject.com/en/3.2/howto/deployment/checklist/)
    

---

> **台南不需要米其林**
> 
> 1. 🏠[專案網址](https://tnfood.pythonanywhere.com/food/)
> 2. 🧑‍💻[專案程式碼](https://github.com/yen900611/TNFood_DJ) 
> 3. 📁[專案文件與鐵人賽文章](https://github.com/yen900611/TNFood)
> 4. 👥參賽團隊 ****[台南巷弄美食獵人](https://ithelp.ithome.com.tw/2022ironman/signup/team/256)****

---