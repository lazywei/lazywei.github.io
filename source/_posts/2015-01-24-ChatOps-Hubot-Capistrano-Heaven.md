title: "ChatOps: Hubot Capistrano Heaven"
date: 2015-01-24 13:57:00
tags: devops
---

一直以來在 Rails 開發中，我都使用 [capistrano](https://github.com/capistrano/capistrano) 作爲自動化部署的工具，他幫我把所有繁雜而瑣碎的部署任務都一次處理好，例如在 `git pull` 最新的 repo 後，要跑個 `bundle install`、有時候還要跑 migration，接着要 precompile assets 等等，最後如果是使用 nginx 搭配 passenger 的方案，還要 `touch tmp/restart.txt` 來通知 web server。

這一切都相當方便 -- 在團隊中所有人都有完整開發環境的前提下（capistrano 是 ruby gem，理所當然需要 ruby environment）。然而事實上，並不是所有團隊成員都有完整的開發環境，例如當我們把前後端分離（透過 API 互相溝通）時，我們就不會希望 F2E 也要裝一份完整的 Rails 環境；再者，透過 capistrano 部署會造成沒有人知道現在有誰在部署、部署情況如何，除非在 capistrano 中再加上 notification to Slack or something else。這對於需要快速 iteration 的新創團隊來說是相當不利的，我們需要的是：
1. 所有人都有辦法 deploy，即使是 art
2. 任何 deploy 發生時，都應該要讓所有人知道這個 deploy 的狀態
3. 所有 deploy 應該都要被完整記錄
4. deploy 應該要可以輕鬆指定要使用哪個 branch，哪個 environment

幸運的，我在前團隊 [iCook](http://icook.tw/) 學到也見識到非常多這種 DevOps 相關的技術（不得不說，iCook CTO [Richard](https://twitter.com/dlackty) 把整個團隊的 DevOps 打造得相當強悍）。跟 Richard 討論及研究了一下後，決定使用如下配置及服務來完成整個部署流程：
1. [Hubot](https://hubot.github.com) 用於在 Slack 聊天室下指令，這樣只要能上聊天室就能部署，即使你是用手機
2. [Github Deployment API](https://developer.github.com/v3/repos/deployments) 用 RESTful 的方式來管理 deployment
3. [Hubot-deploy](https://github.com/atmos/hubot-deploy) + [Heaven](https://github.com/atmos/heaven) 前者讓 hubot 可以操作 Github Deployment API，後者會接受從 Github Deployment API 來的訊息，並負責執行 capistrano

具體一點來說，Heaven 是一個 Rails application，他有一個 `POST /events` 這樣的 route 接收來自 Github Deployment API 的 event，接着利用 resque 來執行實際的 deployment。

這次遇到最大的麻煩是 heaven 和 hubot-deploy 的文件寫得相當差，看了老半天看不出個所以然來，花了不少時間才搞懂這個架構的流程以及一些 tricks，所以在此記錄一下。

--------

**Heaven**
如同前面說的，Heaven 是一個 Rails application，所以需要自己 host，不過他其實也提供了直接放上 heroku 的方式，其中要設定一些環境變數，這邊說明一下他們的用途：
1. `GITHUB_TOKEN` Heaven 在 deploy 完後會利用 gist 作爲他的 STDOUT/STDERR，這個 token 就是讓 heaven 可以 create gist。我個人是另外開了一個 for bot 的 Github account，這樣我的 gist 才不會被塞爆
2. `GITHUB_CLIENT_ID` & `GITHUB_CLIENT_SECRET` 老實說我現在還沒弄清楚他們要幹嘛，不過隨便去申請一個 Github Dev App，然後填上 client id & secret 就好
3. `DEPLOYMENT_PRIVATE_KEY` 因爲執行 cap 的是 heaven，所以 heaven 會需要 ssh access to our server，這個就是他需要用的 private key
4. `SLACK_WEBHOOK_URL` 在 Slack 建立一個 incoming webhook 後填入這裏，這讓 heaven 可以發送 deployment notification 到聊天室，讓所有人都知道部署狀況

大致上 Heaven 這樣丟上 heroku 就可以跑了。

------------------

**Hubot Deploy**

接下來我們設定 hubot deploy，這是一個 hubot script，用 npm 裝完後加入 hubot 的 `external-scripts`，接着比較重要的是建立一個 `apps.json` 在 hubot 的目錄底下，這讓 hubot 知道要怎麼部署哪些 applications，內容大致如下：

```
{
  "hello": {
    "provider": "bundler_capistrano",
    "repository": "lazywei/hello",
    "environments": ["production", "staging"]
  },

  "world": {
    "provider": "capistrano",
    "repository": "lazywei/world",
    "environments": ["production", "staging"]
  }
}
```

這其實就是讓之後我們可以透過 hubot 呼叫 `hubot deploy hello/master ...` 之類的，其中比較需要注意的是 provider，這個欄位的意思是 heaven 要用什麼方式去執行部署的動作，例如 Richard 在 iCook 使用的就是 AWS 的 OpsWorks。

而注意到 provider 又分 capistrano 及 bundler_capistrano，這沒有被寫在文件上，是我折騰了好久才在 source code 中看到的。他們的區別是什麼？首先注意到，heaven 會做的動作是拉下最新的 repo，然後在 repo 中執行 `cap ...` 的指令，但是如果你使用的 cap 與 heaven 的版本不同，就完蛋了，又或者是你的部署需要其他額外的 gems，但是這些 gems 並不在 heaven 的 Gemfile 中，那也會爆炸。好險後來發現 bundler_capistrano，他會在執行 cap 之前建立一個乾淨的 Bundle 環境，然後在這個環境中，安裝「要被部署的 app 的 Gemfile 中被標註爲 heaven or deployment group」的那些 gems，所以我們可以直接把需要被用到的 gems 放進我們原本的 Gemfile 中，以我的例子來說，我就用了最新的 cap 而不是 heaven 預設的 cap 2.9。

另外我的 hubot-deploy 也是放在 heroku 上，有幾個變數需要注意：
1. `HUBOT_SLACK_TOKEN` 這是讓 hubot 可以聽 slack 用的
2. `HUBOT_GITHUB_TOKEN` 這是判斷誰有 deploy 權限，不過建議不要設定這個，請使用 hubot-deploy 提供的指令 `deploy-token:set` 來設定，這個 token 請至你的 github 帳號中 application / personal access 中產生

---------------
**Little Tricks**
雖然整體流程最後看似簡單，但其實中間藏了不少沒被寫在文件上的地雷，這裏記錄一下一些解決的方式或是小技巧：
1. Heaven 放上 heroku 後除了 web dyno 外會開另外兩個 resque dyno，一下就讓你每個月噴 75 美金...但其實這可以透過 `spawn` 來繞過，請參考這篇[黑暗大法](https://coderwall.com/p/fprnhg/free-background-jobs-on-heroku)，感謝 Richard 提供
2. Capistrano 3 會在第一次執行的時候問你要不要讓他們蒐集數據，這會直接讓 heaven 卡住...解法是把 project 底下應該有個 `.capistrano/metric` 寫進 source control (e.g. git)
3. 不知道爲什麼 heaven 在 document 中寫說 cap 的 repo_url 需要用 https 而不是 ssh/git，但是這樣會造成 heaven 在 pull 的時候無法使用 ssh key 來認證，需要寫死 github 的帳號密碼在 `deploy.rb` 中，但這實在太不合理了...結果事實證明用 `git@...` 好像也可以，但是不知道之後會不會突然爆炸就是了

大致上就是這樣了