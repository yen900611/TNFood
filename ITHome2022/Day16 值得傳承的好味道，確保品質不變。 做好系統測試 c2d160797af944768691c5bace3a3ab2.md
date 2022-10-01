# Day16 值得傳承的好味道，確保品質不變。 做好系統測試

editor: 麒麟
tag: Django, Selenium, Testing
進度: 🌕

# 🏁 前言、摘要

<aside>
💡 我們系統大致上已經完成，雖然還有很多值得改進的地方，接下來會一步一步的改好，先求有再求好。
改進過程，要怎麼確保自己改的順利呢？最好的方式就是使用測試案例。今天我們就會使用`Selenium IDE`測試我們的網站，並且開始優化。

</aside>

---

# 💡概念說明

開發過程中，我們多少一定做過測試，但這個測試可不可靠或是有沒有效率就不一定了。從這次的專案一開始，我們每個步驟，都是人工下去做測試。

- 寫好資料模型 → 執行 `makemigrations` 然後 `migrate` ，去管理頁面上看是否有出現。
- 寫好 `urls.py` → 在瀏覽器輸入網址。
- 寫好 `views.py` `template`→ 在瀏覽器輸入網址，和畫面是否符合我們的需要。
- 寫好取得`Place`資料的程式，但是要在`template`使用才知道有沒有抓對。

可以發現有些測試是很沒有效率的，也不可靠。

## 測試的目地，為何要測試？

關於測試，筆者認為有兩個重要的價值。

1. 確認需求，有被確實完成。
    - 這是測試驅動的概念，先確定自己寫的這段程式，要達成什麼目標，再來開始寫程式。
    - 這樣完成的測試案例，直接就說明了系統如何被使用，而且應該有什麼樣的行為。
2. 保護系統在優化的過程，沒有走偏。
    - 在完成功能實作後，有時需要優化效能以及重構程式碼，這時測試案例就能確保修改過程中，程式碼的行為結果沒有改變。

## 測試的分類

測試有分很多種等級，常聽到的單元測試、整合測試，以此專案為例，應該寫出的測試項目為：

- 單元測試：測試篩選店家的功能，邏輯是否正確。
    1. 為篩選的功能寫一個函式
    2. 然後寫測試案例，測試此函式的輸出是否正確。
- 整合測試：測試使用者在操作網站時，是否有誤。
    - 可以使用`Selenium` 來模擬使用者在網頁上的操作。

## 如何決定測試哪些項目

受限於時間人力成本，也不可能每件功能都寫測試。
哪些東西可以寫測試，哪些則是不用呢？

- 變化性大的地方，可以先不用寫測試，測試寫完可能很快就需要修改，這樣也沒有測試的意義。
    - 像是UI變化性相對較高，若要測試頁面上是否出現什麼按鈕等等，則是相對不可行。
- 資料庫變動、環境參數，相對難寫測試，但是又很重要，因此依賴開發人員對於框架與套件的掌握。
- 明確的需求，不會被變更的需求，可以寫測試。
    - 過濾器的功能是明確的，可以寫一個測試。
    - 瀏覽器頁面的切換，是明確的，可以寫一個測試取代人工切換頁面。
- 撰寫測試的方法與成本
    - 撰寫單元測試，需要直接寫程式碼，好處是將確定的功能寫清楚，一方面讓開發者更確定系統的需求，一方面保護系統，但成本相當高。
    - 使用`Selenium IDE`  可以錄製網頁操作，進行初步的整合測試，製作成本低，雖然介面一改變，測試就會失效，但仍有一定的價值。

## 本專案預計的測試

- 參考目前的系統架構圖，本系統預計重構成前後端分離的架構，應該要先為系統加上最外部的整合性測試，確保重構的過程沒有出錯。
    
    ![Untitled](Day16%20%E5%80%BC%E5%BE%97%E5%82%B3%E6%89%BF%E7%9A%84%E5%A5%BD%E5%91%B3%E9%81%93%EF%BC%8C%E7%A2%BA%E4%BF%9D%E5%93%81%E8%B3%AA%E4%B8%8D%E8%AE%8A%E3%80%82%20%E5%81%9A%E5%A5%BD%E7%B3%BB%E7%B5%B1%E6%B8%AC%E8%A9%A6%20c2d160797af944768691c5bace3a3ab2/Untitled.png)
    
- 因此預計先使用 `Selenium IDE`，完成UI的操作測試，再來重構系統。之後再為API加上測試。

## Selenium介紹

1. 是一個網站的測試套件，可以模擬使用者瀏覽網頁的行為，可以使用程式碼編輯開發，也可以使用Chrome的套件來錄製。
    - [https://www.selenium.dev/](https://www.selenium.dev/)
    - [https://medium.com/marketingdatascience/selenium教學-一-如何使用webdriver-send-keys-988816ce9bed](https://medium.com/marketingdatascience/selenium%E6%95%99%E5%AD%B8-%E4%B8%80-%E5%A6%82%E4%BD%95%E4%BD%BF%E7%94%A8webdriver-send-keys-988816ce9bed)
2. 因為沒有要仔細比對網站的元素，只是要確認使用者的操作流程，因此本次專案會直接使用Chrome的套件來錄製，並且在修改後，直接執行測試案例。
    - Chrome 套件 [https://chrome.google.com/webstore/detail/selenium-ide/mooikfkahbdckldjjndioackbalphokd?hl=zh-tw](https://chrome.google.com/webstore/detail/selenium-ide/mooikfkahbdckldjjndioackbalphokd?hl=zh-tw)

---

# 🌟本日成果

![Untitled](Day16%20%E5%80%BC%E5%BE%97%E5%82%B3%E6%89%BF%E7%9A%84%E5%A5%BD%E5%91%B3%E9%81%93%EF%BC%8C%E7%A2%BA%E4%BF%9D%E5%93%81%E8%B3%AA%E4%B8%8D%E8%AE%8A%E3%80%82%20%E5%81%9A%E5%A5%BD%E7%B3%BB%E7%B5%B1%E6%B8%AC%E8%A9%A6%20c2d160797af944768691c5bace3a3ab2/Untitled%201.png)

1. 安裝Selenium
    - 直接到[Chrome擴充功能商店](https://chrome.google.com/webstore/detail/selenium-ide/mooikfkahbdckldjjndioackbalphokd?hl=zh-tw)安裝外掛即可。
    - 安裝完後可以直接開啟，建立專案就能看到此畫面。
    
    ![selenium_begin.png](Day16%20%E5%80%BC%E5%BE%97%E5%82%B3%E6%89%BF%E7%9A%84%E5%A5%BD%E5%91%B3%E9%81%93%EF%BC%8C%E7%A2%BA%E4%BF%9D%E5%93%81%E8%B3%AA%E4%B8%8D%E8%AE%8A%E3%80%82%20%E5%81%9A%E5%A5%BD%E7%B3%BB%E7%B5%B1%E6%B8%AC%E8%A9%A6%20c2d160797af944768691c5bace3a3ab2/selenium_begin.png)
    
2. 開始錄製
    - 設定好起始網址
    - 點選右上紅色REC按鈕可以開始錄製
    - 完成後點紅色REC按鈕，結束錄製
    
    ![selenium_before_test.png](Day16%20%E5%80%BC%E5%BE%97%E5%82%B3%E6%89%BF%E7%9A%84%E5%A5%BD%E5%91%B3%E9%81%93%EF%BC%8C%E7%A2%BA%E4%BF%9D%E5%93%81%E8%B3%AA%E4%B8%8D%E8%AE%8A%E3%80%82%20%E5%81%9A%E5%A5%BD%E7%B3%BB%E7%B5%B1%E6%B8%AC%E8%A9%A6%20c2d160797af944768691c5bace3a3ab2/selenium_before_test.png)
    
3. 執行測試
    - 點選左上即可執行此次的測試。就會開啟一個瀏覽器快速瀏覽頁面，讓我們確認是否運作流暢。
    
    ![index.gif](Day16%20%E5%80%BC%E5%BE%97%E5%82%B3%E6%89%BF%E7%9A%84%E5%A5%BD%E5%91%B3%E9%81%93%EF%BC%8C%E7%A2%BA%E4%BF%9D%E5%93%81%E8%B3%AA%E4%B8%8D%E8%AE%8A%E3%80%82%20%E5%81%9A%E5%A5%BD%E7%B3%BB%E7%B5%B1%E6%B8%AC%E8%A9%A6%20c2d160797af944768691c5bace3a3ab2/index.gif)
    

---

# 🛣️ 心得、結語

<aside>
💡 `Selenium IDE` 除了可以做測試以外，也可以用來重現錯誤，不然發現一個錯誤之後，需要每次都自己按一次，實在耗時耗力。可以直接產生錯誤的操作錄製下來，一邊修改一邊播放，直到錯誤消失為止，是很方便的工具。

</aside>

# 參考資料

1. 各種類型的測試
    - [https://blog.miniasp.com/post/2019/02/18/Unit-testing-Integration-testing-e2e-testing](https://blog.miniasp.com/post/2019/02/18/Unit-testing-Integration-testing-e2e-testing)
2. 使用Selenium 
    - [https://www.selenium.dev/](https://www.selenium.dev/)
    - [https://medium.com/marketingdatascience/selenium教學-一-如何使用webdriver-send-keys-988816ce9bed](https://medium.com/marketingdatascience/selenium%E6%95%99%E5%AD%B8-%E4%B8%80-%E5%A6%82%E4%BD%95%E4%BD%BF%E7%94%A8webdriver-send-keys-988816ce9bed)

---

> **台南不需要米其林**
> 
> 1. 🏠[專案網址](https://tnfood.pythonanywhere.com/food/)
> 2. 🧑‍💻[專案程式碼](https://github.com/yen900611/TNFood_DJ) 
> 3. 📁[專案文件與鐵人賽文章](https://github.com/yen900611/TNFood)
> 4. 👥參賽團隊 ****[台南巷弄美食獵人](https://ithelp.ithome.com.tw/2022ironman/signup/team/256)****

---