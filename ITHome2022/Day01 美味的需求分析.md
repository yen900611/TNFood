# Day01 美味的需求分析


# 🏁 前言

<aside>
💡 台南是美食之都，筆者深受台南美食的囹圄，每到用餐時刻，面對密密麻麻的google Map ，總是不知道該吃什麼比較好。

想要快速篩選餐廳，或是推薦給親朋好友，需要花費很多心力，因此決定趁此機會，開發一個網站來製作自己的美食地圖，並且順便~~學習開發網站的基本流程~~（複習台南的美食）。

本次專案會從整理軟體需求開始，過程中會使用到各種開發工具，並且實作一個前後端分離的系統。後端核心技術會使用Python 的網頁框架 Django。

</aside>

![GoogleMap](Day01%20%E7%BE%8E%E5%91%B3%E7%9A%84%E9%9C%80%E6%B1%82%E5%88%86%E6%9E%90%206468cbfdc02e44ae91521c10c45a49f7/Untitled.png)

---

# 💡概念說明

# 軟體開發流程

不管是什麼樣的開發流程，基本上都會有`需求分析→軟體設計→開發實作→測試驗證→維護`等五個階段，根據專案的規模與性質，會調整各個階段的流向與工作內容。

詳細的說明可以參考此篇[文章](https://tw.alphacamp.co/blog/app-development-process)

本次鐵人賽會從需求分析開始，每日會對應到其中一個流程。

# 需求分析

![需求分析](Day01%20%E7%BE%8E%E5%91%B3%E7%9A%84%E9%9C%80%E6%B1%82%E5%88%86%E6%9E%90%206468cbfdc02e44ae91521c10c45a49f7/Untitled%201.png)

需求分析會需要寫出User Story，列出軟體系統要提供的服務與功能，也描述使用者如何跟此系統互動。任何文件最主要的目標都是溝通，讓每個角色的讀者透過文件取得他們需要的資訊。

舉例：

- 專案經理、產品經理：透過需求文件，了解系統是否能符合市場需求。
- 介面設計師：透過需求文件，了解使用者如何跟系統互動。
- 軟體工程師：根據需求文件，設計軟體的結構並實作。

隨著專案的不同，也會有不同層面的文件，文件太多太雜，反而失去焦點降低溝通效率，文件太簡易，也會增加彼此對系統的理解誤差。

本次專案的需求算是明確，直接參考此文章提到的**JTBD**，來擬訂出初步的需求，希望讓使用者、介面設計者與軟體開發者，對軟體有一致的想像，並進行後續工作。

[產品管理流程中，使用者故事（User Story）常見的三種使用情境](https://medium.com/3pm-lab/3-use-cases-for-writing-effective-user-stories-cd42625fef53)

**Jobs To Be Done (JTBD) — Job Story**

- *When _____ , I want to _____ , so I can _____ .*

JTBD此方法可以清楚地描述使用者在什麼情境使用系統，他們想要解決的問題，以及系統可提供的服務。

---

# 🌟本日成果

# 專案名稱：台南不需要米其林

# 專案概要

Ｑ：臺南哪間店最好吃？

Ａ：我家巷口那間最好吃。

2022 米其林首度將台南納入評鑑，沒有半間入選摘星，卻獲得廣大網友的認同。台南的美食，不需要米其林來認可，在地人更是避之唯恐不及，深怕巷口那間入選後，就變貴又買不到。

本次挑戰就是要~~開發~~（整理）一個台南美食地圖，並協助筆者克服台南美食的選擇障礙（都太好吃了好難抉擇呀～）。

並從中增加自己的~~軟體專業技能~~（身體質量與幸福感）

# 專案規劃

- 專案時程：30天
    - 因為是30天挑戰，因此以10天為一個階段，軟體會有三個階段的演進。
    
    | 專案規劃 | Day01~Day10 | Day11~Day20 | Day21~Day30 |
    | --- | --- | --- | --- |
    | 系統分析 | v |  |  |
    | 需求文件1 | v |  |  |
    | 設計文件1 | v |  |  |
    | Milestone 1  | v |  |  |
    | 需求文件2 |  | v |  |
    | 設計文件2 |  | v |  |
    | Milestone 2 |  | v |  |
    | 軟體部署 |  | v |  |
    | 需求文件3 |  |  | v |
    | 設計文件3 |  |  | v |
    | Milestone 3 |  |  | v |
    

# 需求文件 v1

## 一般使用者

- [ ]  當我打開網站，我想要知道台南有哪些美食，因此我可以直接看到一串清單。
- [ ]  當我點選其中一個項目，我想要知道細部資訊，因此我可以看到這間店的「店家名稱、位置、價格、照片、食物類型、評語…」

---


# 參考資料
1. [https://medium.com/3pm-lab/3-use-cases-for-writing-effective-user-stories-cd42625fef53](https://medium.com/3pm-lab/3-use-cases-for-writing-effective-user-stories-cd42625fef53) 
2. [https://medium.com/3pm-lab/how-to-work-with-engineers-c03fd27ee486](https://medium.com/3pm-lab/how-to-work-with-engineers-c03fd27ee486) 
3. [https://medium.com/3pm-lab/how-to-write-product-requirements-document-5860260716c2](https://medium.com/3pm-lab/how-to-write-product-requirements-document-5860260716c2) 
4. [https://medium.com/%E7%A8%8B%E5%BC%8F%E7%8C%BF%E5%90%83%E9%A6%99%E8%95%89/project-%E8%BB%9F%E9%AB%94%E9%9C%80%E6%B1%82%E7%AE%A1%E7%90%86-%E4%BD%A0%E6%87%89%E8%A9%B2%E8%A6%81%E7%9F%A5%E9%81%93%E7%9A%84%E4%BA%94%E4%BB%B6%E4%BA%8B-6790dc8e7bde](https://medium.com/%E7%A8%8B%E5%BC%8F%E7%8C%BF%E5%90%83%E9%A6%99%E8%95%89/project-%E8%BB%9F%E9%AB%94%E9%9C%80%E6%B1%82%E7%AE%A1%E7%90%86-%E4%BD%A0%E6%87%89%E8%A9%B2%E8%A6%81%E7%9F%A5%E9%81%93%E7%9A%84%E4%BA%94%E4%BB%B6%E4%BA%8B-6790dc8e7bde)