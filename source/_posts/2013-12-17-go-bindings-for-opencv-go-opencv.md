title: Go bindings for OpenCV: go-opencv
tags: ["Golang", "OpenCV"]
date: 2013-12-17 20:45:52
---

I would like to introduce a golang package: [go-opencv](https://github.com/lazywei/go-opencv).

The [original project](https://code.google.com/p/go-opencv) is created by @chaishushan and is hosted on Google code. However, it seems that it has been abandoned. Therefore, I choose to fork this project and host it on Github. Furthermore, I think Github is more popular nowadays, and it's easier to fork/contribute on github than on Google code.

## Introduction

__[go-opencv](https://github.com/lazywei/go-opencv)__ is a Golang binding for [OpenCV](http://opencv.org/), which is used for computer vision.

## Requirement

Before using [go-opencv](https://github.com/lazywei/go-opencv), you should install OpenCV first. On Mac OS X, you can use homebrew:
```
brew install opencv
```

On Ubuntu,
```
sudo apt-get install libopencv-dev
```

## Installation

Please use `go get -u` to get the latest version of [go-opencv](https://github.com/lazywei/go-opencv):
```
go get -u github.com/lazywei/go-opencv
```

## Simple Usage

Here is a simple image resizing example for [go-opencv](https://github.com/lazywei/go-opencv):

```go
package main

import opencv "github.com/lazywei/go-opencv/opencv"

func main() {
	filename := "bert.jpg"
	srcImg := opencv.LoadImage(filename)
	if srcImg == nil {
		panic("Loading Image failed")
	}
	defer srcImg.Release()
	resized1 := opencv.Resize(srcImg, 400, 0, 0)
	resized2 := opencv.Resize(srcImg, 300, 500, 0)
	resized3 := opencv.Resize(srcImg, 300, 500, 2)
	opencv.SaveImage("resized1.jpg", resized1, 0)
	opencv.SaveImage("resized2.jpg", resized2, 0)
	opencv.SaveImage("resized3.jpg", resized3, 0)
}
```

You can find more samples on: https://github.com/lazywei/go-opencv/tree/master/samples

## Contributing

The most important reason I fork and host this project on Github is that I want to make it simple for others to help maintain/develop this project. Therefore, please feel free to open a pull request anytime you want to commit your change. Also, I need some help to make the API document more detail. Please let me know if there is any concern, you can ping me on Github(@lazywei) or on Twitter(@jrweizhang).

Thanks!
