# Day27 分析訪客資訊 使用GA

editor: 麒麟
tag: Data Mining, GA, Google Analytics, django
進度: 🌕

# 🏁 前言、摘要

<aside>
💡 基本的功能已經實作完畢，只剩下陸續增加店家資訊。我們也希望平台可以讓愈多人使用愈好，為了邁向遠大的目標，我們可以做一些準備。接下來幾天內容會包含系統監控、使用者分析、系統管理等等。
今天就是要在我們的網站嵌入Google Analytics。

</aside>

---

# 💡概念說明

在一個商業平台，對營運方來說，會希望知道來訪的對象到底是哪些人，所謂知己知彼百戰百勝。

了解使用者的抱怨與不便，我們就能提供解決方案與更好的服務；了解使用者的喜好與習慣，我們就能確認自身服務的核心架

若是沒有使用會員機制的話，基本上只能取得訪客的IP資訊，或是用多一點工夫追蹤使用者的網站操作習慣，取得的方式如下：

1. 從網站的請求封包可以取得 IP位址。
2. 在網站前端嵌入JS程式，紀錄網站使用者的操作習慣（點擊、滑動、上下頁、離開等等），再透過JS將紀錄送回來。
    - 所以如果有一些網頁運作效能特別高，可能是偷偷地搜集你的資料喔。

對於小型專案，不太可能投入這麼多的資源收集沒有急迫性的資料，因此我們可以使用Google Analytics 簡稱GA。

我們想知道的是：

> 1. 目標對象：訪客長什麼樣子？
2. 客戶開發：訪客從哪裡來？
3. 行為：訪客在網站上做了什麼？
4. 轉換：訪客的轉換情形？
> 
> - 引用自 [https://inboundmarketing.com.tw/google-analytics/](https://inboundmarketing.com.tw/google-analytics/)

GA 就是嵌入一段JS程式碼，並且將使用者的資訊送回GA伺服器，開發者或是平台營運者就能透過GA的介面看到分析資料。

可以分析的項目包含

1. 使用者基本資訊
    1. 國家、地區
    2. 依照情況還可知道使用者的年齡、性別。
2. 網站
    1. 流量
    2. 各個子頁面的分析資訊、
    

---

# 🌟本日成果

![Untitled](Day27%20%E5%88%86%E6%9E%90%E8%A8%AA%E5%AE%A2%E8%B3%87%E8%A8%8A%20%E4%BD%BF%E7%94%A8GA%207189b7b7367644b7b4e31c499b39e55a/Untitled.png)

1. 申請GA服務
    - 只要有google帳號，就能申請使用GA服務
        
        ![ga_manage.png](Day27%20%E5%88%86%E6%9E%90%E8%A8%AA%E5%AE%A2%E8%B3%87%E8%A8%8A%20%E4%BD%BF%E7%94%A8GA%207189b7b7367644b7b4e31c499b39e55a/ga_manage.png)
        
    
2. 建立GA資源
    
    GA 可以申請一個帳戶（可以當作是組織的概念），一個帳戶下可以有多個資源（通常一個網域就是一個資源）。
    
    ![ga_create_account.png](Day27%20%E5%88%86%E6%9E%90%E8%A8%AA%E5%AE%A2%E8%B3%87%E8%A8%8A%20%E4%BD%BF%E7%94%A8GA%207189b7b7367644b7b4e31c499b39e55a/ga_create_account.png)
    
    ![ga_create_account_2.png](Day27%20%E5%88%86%E6%9E%90%E8%A8%AA%E5%AE%A2%E8%B3%87%E8%A8%8A%20%E4%BD%BF%E7%94%A8GA%207189b7b7367644b7b4e31c499b39e55a/ga_create_account_2.png)
    
    ![ga_create_resource.png](Day27%20%E5%88%86%E6%9E%90%E8%A8%AA%E5%AE%A2%E8%B3%87%E8%A8%8A%20%E4%BD%BF%E7%94%A8GA%207189b7b7367644b7b4e31c499b39e55a/ga_create_resource.png)
    
    ![ga_create_resource_2.png](Day27%20%E5%88%86%E6%9E%90%E8%A8%AA%E5%AE%A2%E8%B3%87%E8%A8%8A%20%E4%BD%BF%E7%94%A8GA%207189b7b7367644b7b4e31c499b39e55a/ga_create_resource_2.png)
    
    ![ga_create_resource_3.png](Day27%20%E5%88%86%E6%9E%90%E8%A8%AA%E5%AE%A2%E8%B3%87%E8%A8%8A%20%E4%BD%BF%E7%94%A8GA%207189b7b7367644b7b4e31c499b39e55a/ga_create_resource_3.png)
    
3. 嵌入程式碼
    
    建立完畢後，GA會提供程式碼讓我們可以直接嵌入在網頁中。
    靜態網頁中，需要每個頁面都嵌入，但是在Django 中，我們可以直接嵌入在 `food/base.html` 中，這樣每個頁面都會有同一段程式碼。
    
    ![ga_tutorial.png](Day27%20%E5%88%86%E6%9E%90%E8%A8%AA%E5%AE%A2%E8%B3%87%E8%A8%8A%20%E4%BD%BF%E7%94%A8GA%207189b7b7367644b7b4e31c499b39e55a/ga_tutorial.png)
    
    ```html
    {% block post_script %}
      <!-- Google tag (gtag.js) -->
    <script async src="https://www.googletagmanager.com/gtag/js?id=G-HDSPGV6H9P"></script>
    <script>
      window.dataLayer = window.dataLayer || [];
      function gtag(){dataLayer.push(arguments);}
      gtag('js', new Date());
    
      gtag('config', 'G-HDSPGV6H9P');
    </script>
    {% endblock %}
    ```
    
4. 查看報表
一開始資料還不會彙整到總表之中，但我們可以點選左邊的`即時`，就可以看到30分鐘內的資料。
    
    ![ga_report.png](Day27%20%E5%88%86%E6%9E%90%E8%A8%AA%E5%AE%A2%E8%B3%87%E8%A8%8A%20%E4%BD%BF%E7%94%A8GA%207189b7b7367644b7b4e31c499b39e55a/ga_report.png)
    
    ![ga_report_2.png](Day27%20%E5%88%86%E6%9E%90%E8%A8%AA%E5%AE%A2%E8%B3%87%E8%A8%8A%20%E4%BD%BF%E7%94%A8GA%207189b7b7367644b7b4e31c499b39e55a/ga_report_2.png)
    
    ![screenshot 2022-10-12 17.26.20.png](Day27%20%E5%88%86%E6%9E%90%E8%A8%AA%E5%AE%A2%E8%B3%87%E8%A8%8A%20%E4%BD%BF%E7%94%A8GA%207189b7b7367644b7b4e31c499b39e55a/screenshot_2022-10-12_17.26.20.png)
    

---

# 🛣️ 心得、結語

<aside>
💡 嵌入GA後我們就能更清楚訪客的資訊，目前的資料還不會有什麼用，但在未來有需要的時候就能派上用場。

</aside>

# 參考資料

1. Google Analytic
    - [https://analytics.google.com/](https://analytics.google.com/)
2. 介紹Google Analytic
    - [https://inboundmarketing.com.tw/google-analytics/](https://inboundmarketing.com.tw/google-analytics/)

---

> **台南不需要米其林**
> 
> 1. 🏠[專案網址](https://tnfood.pythonanywhere.com/food/)
> 2. 🧑‍💻[專案程式碼](https://github.com/yen900611/TNFood_DJ) 
> 3. 📁[專案文件與鐵人賽文章](https://github.com/yen900611/TNFood)
> 4. 👥參賽團隊 ****[台南巷弄美食獵人](https://ithelp.ithome.com.tw/2022ironman/signup/team/256)****

---