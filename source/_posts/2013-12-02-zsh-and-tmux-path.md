title: Zsh and Tmux path
date: 2013-12-02 23:04:27
tags: ["Zsh", "Tmux"]
---

I got some weird PATH problems since I started using Tmux. I noticed that the PATH with tmux isn't the same as without tmux. It seems that there are duplicated some paths.

I use rbenv as my ruby version manager, and the duplicated path broke it. The `ruby` command will execute system ruby instead of rbenv's.

I then found the problem is my `zshrc` extend PATH in this way:

```
export PATH=/usr/local/bin:$PATH
```

It means that the PATH will be appended some path to it everytime the `zshrc` be loaded.

Therefore, I hard code the path first in the `zshrc`, and then extend it:

```
export PATH=/Users/lazywei/.rbenv/shims:/Users/lazywei/.rbenv/bin:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/opt/X11/bin:/usr/local/go/bin:/usr/texbin
export PATH=/usr/local/bin:$PATH
```

Then the PATH will reset everytime the `zshrc` got loaded.

This workaround is dirty but is good enough for me.
