# Day10 美食地圖微開張-部署系統

editor: 麒麟
進度: 🌔

# 🏁 前言、摘要

<aside>
💡 經過第一個階段的努力，終於完成了初步的系統，我們要準備將系統部署到網路環境之中。
本次教學會將服務部署在 [https://www.pythonanywhere.com/](https://www.pythonanywhere.com/) ，這個平台可以讓我們架設免費的小網站。

</aside>

---

# 💡概念說明

![Untitled](Day10%20%E7%BE%8E%E9%A3%9F%E5%9C%B0%E5%9C%96%E5%BE%AE%E9%96%8B%E5%BC%B5-%E9%83%A8%E7%BD%B2%E7%B3%BB%E7%B5%B1%209f9150e3a2f74c4abf0ab7cfaa56526b/Untitled.png)

我們已經完成第一階段的需求設計與功能開發，但是開發環境要部署到到正式環境（或是生產環境）會有很多需要考慮的事情。

1. 設置生產環境
    - 在開發環境中，常常會安裝許多套件，若是開發環境已經過度複雜，很容易在部署之後出現意料之外的錯誤。
    - python 環境的管理
        - python 可以使用 virtualenv 來建立一個虛擬環境，讓專案執行在穩定的環境中。
        （沒有建立虛擬環境，我們就容易在本地的python 安裝很多的套件，也意味著專案中可以任意使用其中的套件，這樣我們就不知道我們使用了什麼套件，因此也不知道專案為什麼可以動，為什麼到了其他地方不能動。）
            - [https://docs.python.org/zh-tw/3/tutorial/venv.html](https://docs.python.org/zh-tw/3/tutorial/venv.html)
        - 專案中使用的套件會習慣記錄在 `requirements.txt`  這個檔案中。
            - **強烈建議清楚紀錄套件的版本**，若是沒有紀錄版本，很容易造成開發與生產環境不一致的情況，造成意料之外的錯誤。
            - 筆者的慘痛經驗：`requirements.txt`中沒有註記版本，因此生產環境上安裝了套件的更新版，因此造成毀滅性的錯誤。
            - [https://ithelp.ithome.com.tw/articles/10238370](https://ithelp.ithome.com.tw/articles/10238370)
2. 程式碼部署
本地端更新程式碼後，如何更新至生產環境中，以下介紹幾種方法。
    - 手動更新程式碼（這篇文章的方法）
        
        最簡單的概念就是，登入到生產環境的主機中，並用git或是檔案傳輸等方式將程式碼更新上去。
        
    - 自動化更新程式碼
        
        因為手動上傳檔案，容易少檔案或出現意料之外的錯誤（畢竟我們是人類），因此可以將更新程式碼的步驟標準化（最簡單的方式就是紀錄在一個bash檔案），並讓電腦自動執行即可。
        
        - 可以手動執行 bash 檔案，也有很多方式可以自動觸發去執行bash 檔案，關鍵字可搜尋 `CI/CD`
    - 容器化部署
        
        一句話說明容器：容器就像是貨櫃一樣，將貨物裝貨櫃在裡面，貨物就可以跟著貨櫃到世界各地，容易被移動與搬運。
        容器就是將運行環境與程式碼一起打包，因此容易移植到各式機器上執行。
        我們日後也會再詳細介紹。
        
3. 靜態檔案與資料庫管理
    
    上傳至生產環境後，根據使用者的數量，會重新考量檔案存取與資料庫設計等問題，下述一些基本入門的方法，減少管理負擔。
    
    - 使用獨立的資料庫，並建立備份機制，確保資料不會遺失。
    目前此專案使用SQLiteDB 適合於開發過程使用，尤其Django在移植後，皆會重建資料庫，不容易備份移植，正式環境都會選擇較穩定的資料庫系統。
    - 使用雲端服務的儲存空間，確保靜態檔案的路徑與可靠性。
    如果使用主機的空間儲存靜態檔案，一方面需要顧慮到空間使用率，一方面也會增加主機的流量，因此正式的服務常會選用雲端服務的空間。
    本專案規模還不大，暫時不會採用此方案。
4. 資訊安全
以下列出幾項基礎中的基礎
    - 使用HTTPS加密，確保使用者與伺服器傳輸的敏感內容不會外洩
    - 隱藏系統的路徑，確保伺服器上的檔案，無法被任意存取。

---

# 🌟本日成果

下方會說明如何將服務部署在 [https://www.pythonanywhere.com/](https://www.pythonanywhere.com/) ，這個平台可以讓我們架設免費的小網站。

Django 也有部署指南，提醒開發者要注意的事項，細目非常的多，因為使用了pythonanywhere，我們可以省下許多心力設定網路伺服器、檔案路徑等等，若要部署在正式的VM上可以參考官方文件。

- [https://docs.djangoproject.com/en/3.2/howto/deployment/](https://docs.djangoproject.com/en/3.2/howto/deployment/)

## 註冊

- 註冊教學可以參考 [https://djangogirlstaipei.gitbooks.io/django-girls-taipei-tutorial/content/django/deploy.html](https://djangogirlstaipei.gitbooks.io/django-girls-taipei-tutorial/content/django/deploy.html)
- 注意這裡填入的`username`會是我們建立服務的前綴字喔。
    
    ![sign_up.png](Day10%20%E7%BE%8E%E9%A3%9F%E5%9C%B0%E5%9C%96%E5%BE%AE%E9%96%8B%E5%BC%B5-%E9%83%A8%E7%BD%B2%E7%B3%BB%E7%B5%B1%209f9150e3a2f74c4abf0ab7cfaa56526b/sign_up.png)
    
    - 舉例: [https://tnfood.pythonanywhere.com/](https://tnfood.pythonanywhere.com/)

## 新增Web app

- 點選右邊Web Apps 可以直接建立一個新的網站服務。
- 接著可以看到有許多開發環境可以讓我們選擇，因為筆者使用的是python3.9 Django 3.2，剛好沒有符合的選項，因此我們選用`Manual Configuration`手動設定，讓我們自行安裝虛擬環境與安裝套件。
    
    ![dashboard.png](Day10%20%E7%BE%8E%E9%A3%9F%E5%9C%B0%E5%9C%96%E5%BE%AE%E9%96%8B%E5%BC%B5-%E9%83%A8%E7%BD%B2%E7%B3%BB%E7%B5%B1%209f9150e3a2f74c4abf0ab7cfaa56526b/dashboard.png)
    
    ![web_app.png](Day10%20%E7%BE%8E%E9%A3%9F%E5%9C%B0%E5%9C%96%E5%BE%AE%E9%96%8B%E5%BC%B5-%E9%83%A8%E7%BD%B2%E7%B3%BB%E7%B5%B1%209f9150e3a2f74c4abf0ab7cfaa56526b/web_app.png)
    
- 新增完後可以看到許多的設定欄位，等等會一一填上。

## 上傳程式碼

- 平台提供我們一個資料夾，且可以使用終端機來打指令，因此我們可以直接使用 git 指令來下載程式。之後程式碼更新，也可以進來使用git 更新程式碼即可
- 在`Dashboard`，點選右邊`Console→$Bash`，會開啟一個終端機介面，並打入下方指令。
    
    ![dashboard_console.png](Day10%20%E7%BE%8E%E9%A3%9F%E5%9C%B0%E5%9C%96%E5%BE%AE%E9%96%8B%E5%BC%B5-%E9%83%A8%E7%BD%B2%E7%B3%BB%E7%B5%B1%209f9150e3a2f74c4abf0ab7cfaa56526b/dashboard_console.png)
    
    ![console.png](Day10%20%E7%BE%8E%E9%A3%9F%E5%9C%B0%E5%9C%96%E5%BE%AE%E9%96%8B%E5%BC%B5-%E9%83%A8%E7%BD%B2%E7%B3%BB%E7%B5%B1%209f9150e3a2f74c4abf0ab7cfaa56526b/console.png)
    
    ```bash
    git clone https://github.com/yen900611/TNFood_DJ.git
    ```
    
- 上傳後，可以在`Dashboard→Files` 看到我們的專案檔案
    
    ![dashboard_file.png](Day10%20%E7%BE%8E%E9%A3%9F%E5%9C%B0%E5%9C%96%E5%BE%AE%E9%96%8B%E5%BC%B5-%E9%83%A8%E7%BD%B2%E7%B3%BB%E7%B5%B1%209f9150e3a2f74c4abf0ab7cfaa56526b/dashboard_file.png)
    

## 設定虛擬環境

- 在終端機中使用指令安裝虛擬環境，並安裝`requirements.txt`的套件
    
    ```bash
    cd ./TNFood_DJ
    python3.9 -m virtualenv ./venv
    python -m pip install -r requirements.txt
    source ./venv/bin/activate 
    ```
    

## 修改 設定

- 回到 WebApp 可以看到有許多設定檔。
    
    ![web_app_config.png](Day10%20%E7%BE%8E%E9%A3%9F%E5%9C%B0%E5%9C%96%E5%BE%AE%E9%96%8B%E5%BC%B5-%E9%83%A8%E7%BD%B2%E7%B3%BB%E7%B5%B1%209f9150e3a2f74c4abf0ab7cfaa56526b/web_app_config.png)
    
- 點選 `/var/www/tnfood_pythonanywhere_com_wsgi.py` 即可開啟編輯模式。這個檔案是平台要幫我們建立服務的設定檔，預設的內容中已經有`Django` `Flask` 兩大網頁框架的設定方式。
    - 最重要的概念就是告訴平台專案的路徑，以及 `mysite/settings.py`位置
    
    ```python
    # +++++++++++ DJANGO +++++++++++
    # To use your own django app use code like this:
    import os
    import sys
    #
    ## assuming your django settings file is at '/home/TNFood/mysite/mysite/settings.py'
    ## and your manage.py is is at '/home/TNFood/mysite/manage.py'
    path = '/home/TNFood/TNFood_DJ/mysite'
    if path not in sys.path:
        sys.path.append(path)
    #
    os.environ['DJANGO_SETTINGS_MODULE'] = 'mysite.settings'
    #
    ## then:
    from django.core.wsgi import get_wsgi_application
    application = get_wsgi_application()
    ```
    
- 設定程式碼、虛擬環境
    
    需要在這裡設定好我們程式碼的位置、虛擬環境的位置，平台也很貼心，會判斷此路徑是否有效。
    
    ![web_app_config.png](Day10%20%E7%BE%8E%E9%A3%9F%E5%9C%B0%E5%9C%96%E5%BE%AE%E9%96%8B%E5%BC%B5-%E9%83%A8%E7%BD%B2%E7%B3%BB%E7%B5%B1%209f9150e3a2f74c4abf0ab7cfaa56526b/web_app_config.png)
    
- 做任何的改變後，需要點選 Reload 才可以套用新的程式碼或設定ㄘ
    
    ![web_app_reload.png](Day10%20%E7%BE%8E%E9%A3%9F%E5%9C%B0%E5%9C%96%E5%BE%AE%E9%96%8B%E5%BC%B5-%E9%83%A8%E7%BD%B2%E7%B3%BB%E7%B5%B1%209f9150e3a2f74c4abf0ab7cfaa56526b/web_app_reload.png)
    

## Debug

正常來說上述完成後，網址就可以導向我們的網站了，

但是!!  第一次總是不會那麼順利的，讓我們從錯誤中學習吧

1. 出現錯誤→修改HOST
    
    ![error_host.png](Day10%20%E7%BE%8E%E9%A3%9F%E5%9C%B0%E5%9C%96%E5%BE%AE%E9%96%8B%E5%BC%B5-%E9%83%A8%E7%BD%B2%E7%B3%BB%E7%B5%B1%209f9150e3a2f74c4abf0ab7cfaa56526b/error_host.png)
    
    這是因為我們已經有正式的網域，不是本地的開發環境了，需要在`settings.py` 需要設定 `ALLOWED_HOSTS`，否則會有很多安全性問題。 
    
2. 沒有資料→重新設定資料庫
    
    ![error_no_table.png](Day10%20%E7%BE%8E%E9%A3%9F%E5%9C%B0%E5%9C%96%E5%BE%AE%E9%96%8B%E5%BC%B5-%E9%83%A8%E7%BD%B2%E7%B3%BB%E7%B5%B1%209f9150e3a2f74c4abf0ab7cfaa56526b/error_no_table.png)
    
    這是因為我們部署到新環境，需要在新環境重新建立一次資料庫，也會需要重新建立使用者和新增餐廳的資料。（若是有好的備份機制，就可以更加容易完成這個步驟，之後再介紹了）
    
    ```bash
    python manage.py migrate
    python manage.py createsuperuser
    python manage.py collectstatic
    ```
    
3. 進入admin 發現畫面驟變→設定`static file folder`
    
    ![error_no_css.png](Day10%20%E7%BE%8E%E9%A3%9F%E5%9C%B0%E5%9C%96%E5%BE%AE%E9%96%8B%E5%BC%B5-%E9%83%A8%E7%BD%B2%E7%B3%BB%E7%B5%B1%209f9150e3a2f74c4abf0ab7cfaa56526b/error_no_css.png)
    
    - 沒有出現熟悉的管理介面是因為靜態檔案設定的問題，通常要注意`settings.py`中 `STATIC_URL` `STATIC_ROOT`這兩個變數
    
        ```bash
        # mysite/settings.py 最後記得補上這段
        import mimetypes
        mimetypes.add_type("text/css", ".css", True)
        ```
    - 也要注意`python manage.py collectstatic`這個指令產生的靜態檔位置。
    - 平台則是很好心可以讓我們設定靜態檔案，在WebApp 頁面下方，輸入正確的資料夾位置即可。
        
        ![web_app_static.png](Day10%20%E7%BE%8E%E9%A3%9F%E5%9C%B0%E5%9C%96%E5%BE%AE%E9%96%8B%E5%BC%B5-%E9%83%A8%E7%BD%B2%E7%B3%BB%E7%B5%B1%209f9150e3a2f74c4abf0ab7cfaa56526b/web_app_static.png)
        
    - 恢復正常
        
        ![django_admin.png](Day10%20%E7%BE%8E%E9%A3%9F%E5%9C%B0%E5%9C%96%E5%BE%AE%E9%96%8B%E5%BC%B5-%E9%83%A8%E7%BD%B2%E7%B3%BB%E7%B5%B1%209f9150e3a2f74c4abf0ab7cfaa56526b/django_admin.png)
        

---

# 🛣️ 心得

<aside>
💡 部署的工作相當瑣碎，所幸是大部分都是一次性的工作，之後只需要更新程式碼或是資料庫的資料即可。
這次的部署是最簡易的部署，所謂先求有再就好，還有一些設定的議題之後會陸續補上。
到這邊第一階段就告一段落，接下來會有下個階段的需求和開發，請大家敬請期待。

</aside>

# 參考資料

1. PythonAnyWhere 
[https://www.pythonanywhere.com/](https://www.pythonanywhere.com/) 
2. 如何部署Django 專案到PythonAnyWhere
[https://djangogirlstaipei.gitbooks.io/django-girls-taipei-tutorial/content/django/deploy.html](https://djangogirlstaipei.gitbooks.io/django-girls-taipei-tutorial/content/django/deploy.html)
3. Django 部署指南 
[https://docs.djangoproject.com/en/3.2/howto/deployment/](https://docs.djangoproject.com/en/3.2/howto/deployment/)

---

> **台南不需要米其林**
> 
> 1. [專案程式碼](https://github.com/yen900611/TNFood_DJ) 
> 2. [專案文件與鐵人賽文章](https://github.com/yen900611/TNFood)
> 3. 參賽團隊 ****[台南巷弄美食獵人](https://ithelp.ithome.com.tw/2022ironman/signup/team/256)****

---