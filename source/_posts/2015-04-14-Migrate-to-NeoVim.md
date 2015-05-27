title: "Migrate to NeoVim"
date: 2015-04-14 22:42:18
tags: ["dev", "tools"]
---


Vim 一直以來都是我最喜愛的編輯器，其實我唯一在用的編輯器，當然偶爾還是會有點「七年之癢」想玩玩 Emacs、Atom、Sublime Text，但是最終都還是覺得 Vim 使用起來最順手。

雖然 Vim 是個曠世神作，但是在開發社群上出了一些意見紛爭，例如只有一個人在維護高達 300k 行的 C89，以及這個 Bram 這個主要作者對於 community patching 的態度等，因此在去年約一二月的時候，出現了一個 community fork -- [NeoVim](https://github.com/neovim/neovim)，號稱要重構 Vim，關於一些當時事件的細節可以看看[這篇的推文](https://www.ptt.cc/bbs/Editor/M.1393165951.A.9B0.html)。

這一年來我都有持續在關注 NeoVim 的發展，而這陣子我覺得是一個非常好的入場時機，一些重大的 bugs 都被修了（雖然還是有一些 issues、或是一些爭論），以及我的愛將 [fzf](https://github.com/junegunn/fzf) 也在最近支援了新的 NeoVim features，再加上我的 Vim 今天無預警的突然壞了，所以就毅然決然正式入場。

基本上 NeoVim 的相容性做的很好，我用的差不多將近一百個（突然覺得還真多...）套件都沒出什麼問題，所以如果沒意外的話應該是可以直接 as drop in replacement。

這裏稍微列一下一些需要注意的點或是 features：

1. 不需要 `nocompatible` 了，然後 `encoding=utf-8` 是預設，更多的細節可以參考[這裏](https://github.com/neovim/neovim/wiki/Differences-from-vim)
2. NeoVim 有不一樣的 clipboard 處理方式，可以輕鬆跟 system share clipboard，請參考[這裏](http://neovim.org/doc/user/nvim_clipboard.html#nvim-clipboard)
3. 只要 `PATH` 中有 python interpreter，NeoVim 就能自動支援，不需要再手動 re-build，不過需要裝一下 python package：`pip install neovim`
4. homebrew 可以裝 neovim，但是我不建議用 homebrew 裝，因爲 homebrew 目前在 compile 的時候沒有把 debug mode 關掉，這樣 NeoVim 在遇上 [vim-airline](https://github.com/bling/vim-airline) 的時候會變得非常慢，建議的做法是自己 clone 一份 source code 用 [Optimized build](https://github.com/neovim/neovim/wiki/Building-Neovim#optimized-builds)，然後再把 `$VIMRUNTIME` 設定成 `runtime/` 這個資料夾，最後把 `build/bin` 加入 `$PATH` 就可以了
5. `<C-h>` may not work. If that is the case, please ref to this [workaround](https://github.com/neovim/neovim/issues/2048#issuecomment-78045837)

基本上除了以上這幾點外應該不會再有太多問題了。
