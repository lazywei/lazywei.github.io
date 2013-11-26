title: Install Haskell on Mac via homebrew
date: 2013-08-25 19:13:50
tags: ["Haskell"]
---

As we install many other packages on mac, we can install [Haskell](www.haskell.org) via [Homebrew](http://brew.sh/).
First, search the `haskell`:
```
$ brew search haskell
haskell-platform
```

Install:
```
$ brew install haskell-platform
==> Installing haskell-platform dependency: ghc
...
```

Once installed, you should be able to run:
```
$ ghc
ghc: no input files
Usage: For basic information, try the `--help' option.
```

As well as the Haskell interpreter:
```
$ ghci
GHCi, version 7.6.3: http://www.haskell.org/ghc/  :? for help
Loading package ghc-prim ... linking ... done.
Loading package integer-gmp ... linking ... done.
Loading package base ... linking ... done.
Prelude>
```
