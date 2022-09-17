# Day03 有嚼勁的系統架構

進度: 🌕

# 🏁 摘要

<aside>
💡 對於需求有較清楚的概念後，就要開始思考該如何達成需求。針對這次的專案，筆者想初步畫出軟體架構圖，並開始規劃系統中的資料模型。
希望我們可以設計出一個有彈性（嚼勁）的系統架構，能方便擴充修改（愈嚼愈香）。

</aside>

---

# 💡概念說明

![Untitled](Day03%20%E6%9C%89%E5%9A%BC%E5%8B%81%E7%9A%84%E7%B3%BB%E7%B5%B1%E6%9E%B6%E6%A7%8B%2005e5f2b75e3948dd85a23a9740cb337d/Untitled.png)

對於需求有較清楚的概念後，就要開始思考該如何達成需求。針對這次的專案，筆者想初步畫出軟體架構圖，並開始規劃系統中的資料模型。

## 系統架構圖 Deployment Diagram

[https://medium.com/bucketing/system-design-系統架構基礎-什麼是系統架構-bed1e1323770](https://medium.com/bucketing/system-design-%E7%B3%BB%E7%B5%B1%E6%9E%B6%E6%A7%8B%E5%9F%BA%E7%A4%8E-%E4%BB%80%E9%BA%BC%E6%98%AF%E7%B3%BB%E7%B5%B1%E6%9E%B6%E6%A7%8B-bed1e1323770)

- 用途：描述系統中包含哪些子系統、服務、第三方服務，該如何部署等等，藉由這樣的圖可以畫清系統的分界，增加管理開發的順暢度。
- 常見的網路服務就會包含前端伺服器、後端伺服器、資料庫等三個部分。劃分出來後，就可以個別開發實作，出錯時也比較容易鎖定問題的原因。

## 資料模型 Data Schema

[https://medium.com/bucketing/system-design-系統架構基礎-資料模型-37e62c60e100](https://medium.com/bucketing/system-design-%E7%B3%BB%E7%B5%B1%E6%9E%B6%E6%A7%8B%E5%9F%BA%E7%A4%8E-%E8%B3%87%E6%96%99%E6%A8%A1%E5%9E%8B-37e62c60e100)

- 用途：讓工程師構思資料庫儲存資料的方式，系統逐漸變大的過程中，資料類型也會隨之增多，善用資料模型可以管理好不同資料類型之間的關係，資料庫的效能也可以優化提升。

---

# 🌟本日成果

根據前兩天擬定的需求，我們可以規劃出一個網站系統，在專案的第一個階段，我們先實做一個簡單的網站，因此使用Django就綽綽有餘。Django也內建了SQLite 一個輕量的資料庫，因此我們也不需要額外安裝其他的資料庫系統。

## 系統架構圖 v1

![Untitled](IT%E9%90%B5%E4%BA%BA%E8%B3%BD%20ac998c66ed7d4f36b2bfea77821f3668/%E5%B0%88%E6%A1%88%E6%96%87%E4%BB%B6%2062ccf9cd84a3418ab9c39636cc7110f8/Project%20Doc%20v1%205b7418dedf7d4ac0bd8005462bbd0b19/Untitled.png)

第一個階段，我們可以直接使用 Django 來接受使用者的請求，並直接回傳網頁結果給使用者的瀏覽器，因此我們只需要開發Web這個區塊即可。

> 淡藍色表示運行軟體服務的機器，綠色表示部署的最小單位，黃色則是服務中的一個模組，因為Django跟SQLite會同時部署，因此放在同一個綠色框框中。
> 
> 
> 未來引入PostgresSQL、Redis等不同的資料庫，就會用另外一個綠色框包起來，這也是微服務的概念。
> 

## 資料架構圖 v1

因為美食地點包含餐廳、咖啡廳、麵包店、甜點店、巷口那家店，甚至是不起眼的住宅家，因此物件就用`Place`來命名。

網站上也要顯示照片，一個地點會不只一張照片，因此建立一個`Photo`的資料類型，來儲存圖片資訊。

`Place` 和 `Photo` 之間也會有關聯，因此需要額外建立一個表格。

成果如下

![Untitled](IT%E9%90%B5%E4%BA%BA%E8%B3%BD%20ac998c66ed7d4f36b2bfea77821f3668/%E5%B0%88%E6%A1%88%E6%96%87%E4%BB%B6%2062ccf9cd84a3418ab9c39636cc7110f8/Project%20Doc%20v1%205b7418dedf7d4ac0bd8005462bbd0b19/Untitled%201.png)

可以看到`Place`有許多屬性，後面會加上此屬性的資料型別，常見的有數字、字串等，在資料庫中甚至會需要設定字串的最大長度，不同的資料庫系統可支援的資料型態有些差異，這裡就先使用text 來表示不固定的字串。

而`Photo`有一個FK(foreign key)，此欄位會儲存一個地點的PK，這樣也表示兩個資料模型是一個1對多的關係，一個地點會有多張照片，一張照片則只能代表一個地點。

---

# 🎶 Drawio 小技巧

只要把一個圖形的`**container**`屬性打勾，裡面就可以裝其他圖型，並且可以跟著拖曳囉。

![截圖 2022-09-14 23.14.15.png](Day03%20%E6%9C%89%E5%9A%BC%E5%8B%81%E7%9A%84%E7%B3%BB%E7%B5%B1%E6%9E%B6%E6%A7%8B%2005e5f2b75e3948dd85a23a9740cb337d/component_is_not_container.png)

![截圖 2022-09-14 23.14.21.png](Day03%20%E6%9C%89%E5%9A%BC%E5%8B%81%E7%9A%84%E7%B3%BB%E7%B5%B1%E6%9E%B6%E6%A7%8B%2005e5f2b75e3948dd85a23a9740cb337d/component_is_container.png)

- 參考資料
    1. [Draw.io](http://Draw.io) 
    2. [Draw.io教學](https://www.youtube.com/watch?v=CU0ZhMoXz7k)
    3. [https://namepluto.com/關聯式資料庫的鍵值類型-pkfk-是什麼？/](https://namepluto.com/%E9%97%9C%E8%81%AF%E5%BC%8F%E8%B3%87%E6%96%99%E5%BA%AB%E7%9A%84%E9%8D%B5%E5%80%BC%E9%A1%9E%E5%9E%8B-pkfk-%E6%98%AF%E4%BB%80%E9%BA%BC%EF%BC%9F/)