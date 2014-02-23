title: Godir with direnv for golang projects
date: 2014-02-23 12:24:29
tags: ["golang", "direnv"]
---

Last time, I described how I use `direnv` to manage my multiple golang projects ([link](http://blog.lazywei.com/2014/02/10/use-direnv-to-manage-your-golang-projects/)). It's convenient to me, but repeating the steps everytime is somehow annoying.

Therefore, I'd like to show my script to do those steps automatically today.
I puts the shell script [here](https://github.com/lazywei/dotfiles/blob/master/bin/godir).
You can download it into your `~/bin` and then add your `~/bin` to your `$PATH`.

Usage:
```
godir project_name
```

What the script does are the following things:

- create a folder named `project_name`
- add direnv settings
- create src folder

There are, however, other problems need to deal with.

First, you should create a special project (e.g. `tools`), and add it to your `$PATH`. Then you can put general tools like [gocode](https://github.com/nsf/gocode), [textql](https://github.com/dinedal/textql) into this folder.

Second, some useful packages like [go-spew](https://github.com/davecgh/go-spew) should be installed in every projects. I think I will write some code to do it automatically next time.

Third, the `src` folder (`$PWD/src/github.com/lazywei`) is fixed now. I will make it more convenient for other to use. But for now, you should change it manually.
