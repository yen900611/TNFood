# Day25 系統監控與警示

editor: 麒麟
tag: Sentry, django, maintenance, monitor
進度: 🌕

# 🏁 摘要

<aside>
💡 改善完前端的頁面，要實作新功能之前，有一些維護事項或是小工作可以先做，因為系統功能逐漸變多，需要加上一些監控機制，讓我們可以即時知道錯誤，及時修復。
今天會介紹如何使用 `Sentry` 來監控系統。

</aside>

---

# 💡概念說明

當網站平台上線後，我們會需要關注幾個重點：

1. 可用性問題，伺服器是否在運行中？
    - 需要設計備援機制以及重啟機制，可以使用負載平衡器、Docker、K8S等軟體來達成，在本系列文中不會介紹到。
2. 效能監控議題，網站是否正常運行，是否有異常流量
    - 基本概念是要在伺服器站點使用監控軟體，並將伺服器的資料收集起來再視覺化顯示出來。再接著設置系統異常的條件，若出現異常則透過信件簡訊等方式通知維護者。
    - 雲端主機都會有VM的效能分析，也可以設置警示系統。
    - 另外可使用效能監控服務，如 `Prometheus` `Grafana` 來協助收集網路流量、CPU、RAM的使用量，與設置警示。
3. 網站運行過程中是否會出現Bug，出現了應該如何處理？
    - 系統難免會有預期以外的狀況產生，進而造成平台發生錯誤，使用者得到預期以外的結果。
    - 有愈完整的出錯紀錄，就愈能協助開發者改善系統，因此需要建立錯誤回報與收集系統。
    - 通常會在程式碼嵌入`Logger` 收集系統的錯誤
        - PythonAnywhere會提供`error.log`給開發者查閱。
    - 也可以使用錯誤回報軟體，如`Sentry`協助管理錯誤。

# Sentry

Sentry 是一個開源的系統錯誤監控軟體，可以嵌入在各種語言各種框架環境之中，提供會員免費的額度與定量空間，協助會員收集錯誤，也可以協助監控系統效能。若是有能力自行管理主機，也能下載原始碼自行部署。

詳細介紹可參考

[https://blog.starrocket.io/posts/open-source-saas-sentry-helps-developers-to-monitor-application-and-track-error/](https://blog.starrocket.io/posts/open-source-saas-sentry-helps-developers-to-monitor-application-and-track-error/)

---

# 🌟本日成果

![Untitled](Day25%20%E7%B3%BB%E7%B5%B1%E7%9B%A3%E6%8E%A7%E8%88%87%E8%AD%A6%E7%A4%BA%20a88882a5753e4bdcbd8079a2d20f6e3c/Untitled.png)

1. 申請Sentry 帳號
    - 可以直接使用Google 第三方登入
2. 建立組織（Organization）
    - 組織下可以有數個專案，這裡就以本專案的名稱作為組織名稱。
    - 這裡實際使用會建立兩個專案，一個監控離線環境，一個監控正式環境。
3. 建立專案（Project）
    - 這裡可以看到多種平台都可以適用，可以依照自身需要，我們就選擇Django。
        
        ![create_project.png](Day25%20%E7%B3%BB%E7%B5%B1%E7%9B%A3%E6%8E%A7%E8%88%87%E8%AD%A6%E7%A4%BA%20a88882a5753e4bdcbd8079a2d20f6e3c/create_project.png)
        
    - 設定專案名稱
        
        ![create_project_2.png](Day25%20%E7%B3%BB%E7%B5%B1%E7%9B%A3%E6%8E%A7%E8%88%87%E8%AD%A6%E7%A4%BA%20a88882a5753e4bdcbd8079a2d20f6e3c/create_project_2.png)
        
    - 簡易教學
    官網會提供簡易的教學，教我們如何使用Sentry，要注意的是每個專案會有一個專屬的DSN，**設定同一個DSN，就會由同一個Sentry專案來收集資訊**，這點要注意一下。
        
        ![tutorial.png](Day25%20%E7%B3%BB%E7%B5%B1%E7%9B%A3%E6%8E%A7%E8%88%87%E8%AD%A6%E7%A4%BA%20a88882a5753e4bdcbd8079a2d20f6e3c/tutorial.png)
        
    - `traces_sample_rate`是要設定效能收集的頻率，生產環境中會調小。
4. 觸發第一個錯誤，並查看錯誤資訊。
    - 官網會教我們觸發一個 1/0 的錯誤
    在本地會出現下圖
        
        ![error.png](Day25%20%E7%B3%BB%E7%B5%B1%E7%9B%A3%E6%8E%A7%E8%88%87%E8%AD%A6%E7%A4%BA%20a88882a5753e4bdcbd8079a2d20f6e3c/error.png)
        
    - 在Sentry 專案中則會收到Issue
        
        ![issue.png](Day25%20%E7%B3%BB%E7%B5%B1%E7%9B%A3%E6%8E%A7%E8%88%87%E8%AD%A6%E7%A4%BA%20a88882a5753e4bdcbd8079a2d20f6e3c/issue.png)
        
    - Issue 中會詳細說明錯誤發生的地方，瀏覽器、網站路徑、Cookie、Session等，跟Djano預設的錯誤資訊差不多。
        - 在生產環境中不宜將Debug資訊讓使用者看到，透過Sentry能將錯誤收集整理起來，協助我們查閱。
        
        ![error_info.png](Day25%20%E7%B3%BB%E7%B5%B1%E7%9B%A3%E6%8E%A7%E8%88%87%E8%AD%A6%E7%A4%BA%20a88882a5753e4bdcbd8079a2d20f6e3c/error_info.png)
        
5. 設定環境變數
    - 因為環境不同，DSN有所不同，因此這裡使用環境變數來設定不同錯誤，應該要送去不同的專案。
    
    ```python
    import sentry_sdk
    from sentry_sdk.integrations.django import DjangoIntegration
    SENTRY_CLIENT_DSN = os.getenv('SENTRY_CLIENT_DSN')
    sentry_sdk.init(
        dsn=SENTRY_CLIENT_DSN,
        integrations=[
            DjangoIntegration(),
        ],
    
        traces_sample_rate=0.01,
        send_default_pii=True
    )
    ```
    

---

# 🛣️ 心得結語

<aside>
💡 安裝與設定Sentry相當簡單，當收集到錯誤時，也會寄信到信箱通知我們，而且會將錯誤資訊紀錄得非常清楚，提供我們非常好的除錯資訊。雖然免費版只能一個人單獨使用，但對於初期的專案來說已經是相當實用。

</aside>

# 參考資料

1. Sentry
    - [https://sentry.io/welcome/](https://sentry.io/welcome/)
    - [https://docs.sentry.io/](https://docs.sentry.io/)
    - [https://github.com/getsentry/](https://github.com/getsentry/)
2. Sentry介紹
    - [https://blog.starrocket.io/posts/open-source-saas-sentry-helps-developers-to-monitor-application-and-track-error/](https://blog.starrocket.io/posts/open-source-saas-sentry-helps-developers-to-monitor-application-and-track-error/)

---

> **台南不需要米其林**
> 
> 1. 🏠[專案網址](https://tnfood.pythonanywhere.com/food/)
> 2. 🧑‍💻[專案程式碼](https://github.com/yen900611/TNFood_DJ) 
> 3. 📁[專案文件與鐵人賽文章](https://github.com/yen900611/TNFood)
> 4. 👥參賽團隊 ****[台南巷弄美食獵人](https://ithelp.ithome.com.tw/2022ironman/signup/team/256)****

---