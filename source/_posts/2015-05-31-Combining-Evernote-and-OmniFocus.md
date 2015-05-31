title: Combining Evernote and OmniFocus
tags:
  - tools
  - productivity
date: 2015-05-31 15:05:33
---


大概從去年年底開始，我就成了 Evernote 的重度使用者，對我來說 Evernote 就是一個什麼都可以記錄的地方，有些人稱呼 Evernote 爲第二個大腦，我覺得挺適切的。

不過我發現 Evernote 也許在很多地方上表現得不錯，但他並不適合做一個 TODO list，雖然他有待辦清單的功能，但是跟一些成熟的 TODO list 軟體比起來真的差多了，而且 Evernote 的 TODO items 很有可能會散落在各個不同的筆記本裏面，很難有一個可以一口氣看到所有統整畫面的地方。
不幸的是，這點對我來說很重要，在安排事情的 priority 時，我習慣可以縱覽全部的待辦事項。再者，我喜歡的工作流是 GTD，而 Evernote 在這個 context 下是完全不適用的。或者應該說，Evernote 其實比較像一個用來記錄各種想法、以及想法的細節的地方，而不是一個記錄「待辦事項」的地方。

而說到 GTD，在 Mac 上最好用的大概就是 OmniFocus 了，他可以按照很多 GTD 的 convention 來分類待辦事項，然後也可以設定 due date（這點在 evernote 上實在不是很好用），除此之外更可以設定每個專案的 review cycle，固定一陣子 review 某個專案的內容等等。不過沒有完美的軟體，OF 在記錄細節上表現的並不好，例如當我安排了一個「做研究」的待辦事項後，我會想要在記錄一些當下的想法，這點在 OF 上就很難做，而且如果真的在 OF 上記錄了這些想法，那這些細節就會同時散落在 OF 和 Evernote 上了（因爲我也會記錄一些東西在 EN），身爲一個工程師，我還是比較希望能有個 single source of truth，我也希望我工作的思考歷程能被記錄下來，而不是隨着 item checked 之後就消失了。

總結來說 EN 是記錄事情的王者，而 OF 是安排事項的王者，所以我後來想到了一個方法，可以讓這兩個工具同時做好自己擅長的事情，然後把剩下的交給對方：

1. 把細節、思考、工作的東西都記錄在 EN，例如我會在一個 note 上寫關於做某個研究需要注意的一些事項、或是實驗該怎麼設計比較好
2. 利用 EN 的記事鏈接，複製該篇 note 的 link，然後把這個 link 貼上到 OF 的 note section，這樣就可以在處理 OF 上的待辦事項時，cross reference 到 Evernote 中的細節

實際做起來大概是長這樣：

OmniFocus：
![](img/2015/5/31-omnifocus.png)

Evernote：
![](img/2015/5/31-evernote.png)
