title: Use direnv to manage your golang projects
date: 2014-02-10 11:32:00
tags: ["direnv", "golang"]
---

I used to put all my golang projects in to a directory so I only needed a single `GOPATH` and `PATH` configuration in my `.zshrc`.
However, this makes things complicated. There are too many projects in `GOPATH/src`, and it's hard to manage them.

Fortunately, I found [direnv](https://github.com/zimbatm/direnv). For more detail, please read its README. I'm going to show how I use direnv to help me organize my golang projects.

1. Install direnv
2. `mkdir ~/GoProjects` and `cd ~/GoProjects`
3. `mkdir demo_project` and `cd demo_project`
4. `direnv edit .`
5. Now direnv will open `.envrc` in your `$EDITOR`. We can then put
```
export GOPATH=$PWD
export PATH=$PWD/bin:$PATH
```
  into this files.
6. `go get ...`

Now `~/GoProjects/demo_project` is an independent workspace, enjoy it. Repeat the above steps to organize your projects.
