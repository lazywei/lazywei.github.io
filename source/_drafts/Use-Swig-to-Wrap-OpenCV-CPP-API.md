title: "Use SWIG to Wrap OpenCV C++ API"
tags: [dev, opencv, golang]
---

許久以前我 fork 了一份 [Golang 的 OpenCV bindings](https://github.com/lazywei/go-opencv) 在 GitHub 上，但是其實平常幾乎沒怎麼用到，也因此幾乎沒有在維護，只有偶爾收收 Pull Request 而已。

最近剛好工作上需要將 Python+OpenCV 的 Code 轉成 Go，於是想到可以使用 `go-opencv` 來寫，不過原先 Python 使用的是 OpenCV 3 的 API，而 OpenCV 在 2.x 之後就已經遷移到 C++ API 了，也就是已經沒有再繼續開發 C 的 API，而不幸的是 CGO 只支援 C、並不支援 C++，如果硬要用 C++，那麼只有兩個方法：

1. 幫要用到的 C++ function 寫一份純 C 的 interface，其實就是自己手刻 wrapping 的意思
2. 用 SWIG (simplified wrapper and interface generator) 來自動產生 wrapping code

經過一些研究和考量，我最終決定使用 SWIG。其實我對 SWIG 並不熟，充其量也是亂用而已，詳細的介紹或許直接看官網是最快的，這篇主要記錄一下我怎麼組合 SWIG / Go / C++ / External Libraries。

## SWIG

大致上 SWIG 就是一個幫你產生能在其他語言使用 C/C++ API 的wrapper 的產生器，目前支援許多 script language，諸如 Ruby / Python 等，而在 SWIG 2.0 後也支援了 Go。

SWIG 提供很多語法以支援各種 C++ 的特性，例如 template 或是 STL，不過當要 wrap 的 header file 很乾淨、也很簡單的時候，可以直接使用懶人方式：


```
%module go_calib3d

%{
#include "go_calib3d.hpp"
%}

%include "go_calib3d.hpp"
```

這裏做的事情很簡單，利用 `%module` 定義包裝好的 go package name、利用 `%{...%}` 來確保 wrapper 有 include 正確的 header file、利用 `%include` 來直接 parse header file 產生 wrapper code。  
像這樣的一個 SWIG file 就可以把 `go_calib3d.hpp` 這個 header file 裏面定義的東西包成可以透過 Go 存取的 package。


## Go package 與 SWIG 的目錄結構

這次在寫 OpenCV 的 wrapper 時，我自己摸索出來的檔案結構大概是這樣：

```
- example.cpp
- example.hpp
```

這兩個是我們要包裝起來的原始程式碼，`.hpp` 是 header file，而所有的實作都在 `.cpp` 裏面。

```
- example.swigcxx
```

這就是要放我們寫的 SWIG，其中要注意檔名，當檔名是 `.swigcxx`，我們在透過 `go build` 來編譯時、`go` 就會自動在呼叫 SWIG 時帶上 `-c++` 這樣的參數，讓 SWIG 知道我們現在要包的是 C++。
使用 `go build` 有個好處：不需要像 SWIG 網站上的教學那樣，自己手動使用 `6g`、`6l` 等 go tools，如果跟 Go 相關的環境變數、如 `GOROOT` 等沒設置好的話、手動使用這些 tools 真的是相當麻煩，我就在這上面吃的不少虧，而使用 `go build` 的話則可以一鍵編譯，任何設定都不需要。

不過要使用 `go build` 的話，我們要有一個 go package 來給他編譯，不然他會不知道要 build 什麼，所以我們可以在同個目錄底下新增一個 `go_calib3d.go` 的檔案，並且在裏面定義我們的 "dummy" package：

```go
package go_calib3d
```

除此之外這個檔案不需要再添加任何其他的東西，這樣我們就可以直接使用 `go build` 來編譯整個 package，而 `go build` 也會自動使用 SWIG 等相關的 tools。

## CFLAGS

上述所說使用 `go build` 的方式固然方便，但是也失去的一些彈性，例如我的 `go_calib3d.cpp` 裏面用到了 OpenCV 的 header files，所以如果不在編譯 C++ 的時候加上 include path 的話，compiler 就會找不到 OpenCV functions 的定義而報錯；抑或我想用 c++11 的語法，但是 `go build` 預設使用的 compiler 並沒有帶上這個參數。  
如果不解決這個問題，那麼 `go build` 勢必無法使用，好險我們其實可以偷偷使用 CGO 的方式來偷渡這些 flag，解法是把剛剛的 `go_calib3d.go` 裏面改成：

```go
package go_calib3d

// #cgo CXXFLAGS: -std=c++11
// #cgo darwin pkg-config: opencv
import "C"
```

這樣當 `go build` 看到 `import "C"` 的時候，就會用處理 CGO 的方式來對待他上面的那些 CGO comments，也就是會自動幫我們加上想要使用的 flags 了。

6. Pointer / Class
7. Typemap
8. nested header including -- additional include
9. typedef 用 %template 來解決
10. 可以只 wrap 某部份的 code
11. 利用 %include 來 modulize
12. reference? 直接拿來用吧！
13. %template 直接當 rename 用，再多包一層 interface 
14. 注意 SWIG <-> Golang 的 type 轉換，若是 *float64 要用 type assertion
