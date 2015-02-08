---
title: 'Hashing Password and Timing Attack'
date: 2015-02-02 11:59:00
tags: dev
---

今天在實作 Token-Based 登入系統的時候碰到了一點小問題：原本在 Rails 中的 User System 是使用 [Devise](https://github.com/plataformatec/devise)，但是我們的 API Server 並不在 Rails 底下，也就是說我沒有辦法直接存取 Devise；在這樣的情況下，我就必須要自己將「把 plain text 的密碼做 hash 並跟資料庫中的密碼比較」的這個步驟實作出來，並且還要跟 Devise 做的相容。

於是我和 [Godfat](godfat.org) 就開始看 Devise 是怎麼做的，結果發現 Devise 做了一件有趣的事情 -- 他在 compare hashed password 的時候用了一個 `secure_compare`，看了一下 source code 後發現，使用這個特製 `secure_compare` 的原因是爲了達成 **Constant time (in password length) Comparison**，而想要 constant time 則是爲了預防 timing attack。

所謂的 timing attack 及 contant time comparison 可以參考[這篇](http://security.stackexchange.com/questions/46212/does-bcrypt-compare-the-hashes-in-length-constant-time)，大致上就是說利用「比較兩個 hashed string」所花費的時間來猜出正確的 hashed string，例如當你發現某個 hashed string 在比較的時候比人家慢了 2ms，那有可能這個 hashed string 的開頭和正確的 hashed string 的開頭是一致的，這就是 timing attack。所以爲了預防 timing attack，使用「不論在任何情況下都用一樣的時間做比較」的比較函數就相當重要了。

但是這個情況在 devise 中就有點 tricky，首先，devise 使用的是 bcrypt，這個 hash function 是有帶 salt 的（類似密鑰的東西），在沒有 salt 的情況下，是幾乎不可能得知 hash function 是如何 mapping 的 -- 理論上 salt 是不會 leak 的。而前面所說的 timing attack 是建立在攻擊者知道 hash function 如何 mapping 的基礎上，如果攻擊者沒有 salt，那麼比較時的時間差就不會泄漏任何訊息給攻擊者。詳細的討論可以看在 [bcrypt-ruby 上的討論](https://github.com/codahale/bcrypt-ruby/pull/43)，在有 salt 的情況下，使用 slow comparison 其實沒什麼意義，所以 devise 大概是做了白工...