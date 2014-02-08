title: XeLaTex with vim and texshop
date: 2013-10-28 22:50:18
tags: ["Vim", "Latex"]
---

##XeLaTexSince
Sometimes I need to write 中文 in my tex file, the most convenient way is to use [XeLaTex](http://en.wikipedia.org/wik/XeTeX).
It's really easy to configure the XeLaTex to deal with 中文, just add the following lines to your tex file:
```tex
\usepackage{fontspec,xltxtra,xunicode}
\setromanfont{Apple LiSung Light}
```

You can find my template tex file from [here](https://gist.github.com/lazywei/6606677)

##Vim
There is a omni functional vim plugin for latex: [vim-latex](http://vim-latex.sourceforge.net/). However, it seems too complicated to me. All I need is just some key mappings, and I put the config into a separated plugin: [vim-language-specific](https://github.com/lazywei/vim-language-specific). Then you can use `<leader>xe` to save file and compile it with xelatex.

##TexShop
I originally used [TexMaker](http://www.xm1math.net/texmaker/). It's good but it fails on Mavericks (OS X 10.9). Fortunately, I found another awesome software: [TexShop](http://pages.uoregon.edu/koch/texshop/). It is a lightweight but powerful software, and you can configure it to use external editor, which is my favorite. Just check the 'Auto Updating' in preference panel. Then you can simply write and compile tex in vim, and view it in TexShop.
