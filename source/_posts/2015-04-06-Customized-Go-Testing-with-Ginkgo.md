title: "Customized Go Testing with Ginkgo"
date: 2015-04-06 11:43:09
tags:
---


這次在寫 [go-opencv](https://github.com/lazywei/go-opencv) 的時候遇到一些測試上的小問題：我用了 gonum 的 [mat64](https://godoc.org/github.com/gonum/matrix/mat64) 來做基礎矩陣操作，所以很多測試上都需要直接對 `mat64.Dense` 測試，而 `mat64.Dense` 底下的資料是 `[]float64`，所以我需要的是可以直接對 `[]float64` 測試的套件。然而大部分的測試框架內建的 assertion 都頂多到 `float64` 的 approimately comparison，所以勢必要自己寫 assertion。

一開始我用的是 [testify](https://github.com/stretchr/testify)，是個相當簡潔易用的測試框架，他提供了 `InDelta` 這樣的 assertion 來比較兩個 `float64` 是不是在容許誤差內，於是我就自己多寫了一個 [`InDeltaSlice`](https://github.com/lazywei/testify/commit/f0b02af48e5ee31c78b949e9ed67c37e08d1a897) 來比較兩個 `[]float64`，如此一來我就可以用

```go
assert.InDeltaSlice(t,
		[]float64{-5.47022369e+03, 0.00000000e+00, 9.59500000e+02, 0.00000000e+00},
		mat.Row(nil, 0), tol)
```

的方式來做測試，但其實我更想要的，是直接吃兩個 `mat64.Dense` 的 assertion，而不用像我這樣自己手動取每個 Row 或 Column 出來比較。

這時候我發現了 [Ginkgo](http://onsi.github.io/ginkgo) 以及 [Gomega](http://onsi.github.io/gomega)，前者是一套 BDD 的測試框架，後者則是搭配 Ginkgo 使用的 matchers。Gomega 的好處是 matcher 的 interface 訂得相當彈性，所以可以很輕鬆的自己加上[自定義的 matcher](http://onsi.github.io/gomega/#adding-your-own-matchers)，於是我就寫了一個 package [gomegamat64](https://github.com/lazywei/gomegamat64)，打算把之後跟 `mat64` 相關的 matcher 都寫進來，目前提供了一個 `AllClostTo` 的 matcher，讓我可以直接這樣做：

```go
pt3 := mat64.NewDense(3, 1, []float64{1, 3, -2})

Ω(pt3).Should(gomegamat64.AllCloseTo(
        mat64.NewDense(3, 1, []float64{1, 3, -3}),
        1e-6,
        false,
))
```

而且更棒的是，如果在一個很大的矩陣中，matcher 只告訴我兩個矩陣不相等，而沒有告訴我是哪個 Row/Column 不相等的話，其實要 debug 是相當困難的，而 Gomega 定義良好的 interface 可以讓我直接在 not match 的時候給出有用的資訊，例如這個 `AllCloseTo` 如果 not match 的話，會給出這樣的 error message

```
Expected -2 to close to -3: pos=(2, 0), tol=1e-06, relative=false
```

而且如果兩個矩陣的 dimensions 不一致的話，這個 matcher 也可以擋下來，這樣就不用再自己手動檢查了！
