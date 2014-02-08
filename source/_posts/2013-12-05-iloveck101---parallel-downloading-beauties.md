title: iloveck101 - parallel downloading beauties
date: 2013-12-05 10:18:17
tags: ["Golang"]
---

@tzangms wrote an amazing python script few days ago, [iloveck101](https://github.com/tzangms/iloveck101). It is a tool for downloading all the images in the post of CK101 卡提諾論壇 (e.g. http://ck101.com/thread-2864249-1-1.html). Then we don't need to wait for the lazy image loading and those annoying ads.

As soon as @tzangms release this cool tool, there are many others clone in other languages show up. For example, there is a node.js version: https://github.com/clonn/iloveck101.

I also rewrite it in golang: https://github.com/lazywei/iloveck101. Thanks goroutine, this version allows you to download images parallely.

### Install

`go get github.com/lazywei/iloveck101`

### Usage

`iloveck101 -u [url] -w [workers number]`

### Example

`iloveck101 -u http://ck101.com/thread-2864249-1-1.html -w 50`

Then it will create a folder in `~/Pictures/iloveck101` which contains all the beauties' images :)
