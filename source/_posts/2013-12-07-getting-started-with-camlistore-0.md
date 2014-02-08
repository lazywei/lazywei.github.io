title: Getting started with Camlistore (0)
date: 2013-12-07 16:37:25
tags: ["Golang", "Camlistore"]
---
> Camlistore is your personal storage system for life.

If you have never heard about [Camlistore](http://camlistore.org/) before, you can checkout the video demo on the website.

I would like to write down the process of setting up a Camlistore server and how to using it and help more people know how wonderful and useful Camlistore is.

## Install GO
If you haven't had GO installed, you can use [gvm](https://github.com/moovweb/gvm) to install it.
Please read the requirement section of its [README](https://github.com/moovweb/gvm#mac-os-x-requirements), and then
```
bash < <(curl -s https://raw.github.com/moovweb/gvm/master/binscripts/gvm-installer)
gvm install go1.2
gvm use go1.2
```

## Build Camlistore
After installing GO, it's time to build Camlistore. First, download zip from http://camlistore.org/download or use git clone just like me:
```
git clone https://github.com/bradfitz/camlistore
```
Install:
```
cd camlistore
go run make.go
```

If you use `homebrew` as your package management on OS X, you may get error when building `go-sqlite3`:
```
camlistore.org/third_party/github.com/mattn/go-sqlite3
# pkg-config --cflags sqlite3
Package sqlite3 was not found in the pkg-config search path.
Perhaps you should add the directory containing `sqlite3.pc'
to the PKG_CONFIG_PATH environment variable
No package 'sqlite3' found
exit status 1
```
The solution is use this command instead:
```
PKG_CONFIG_PATH=/usr/local/Cellar/sqlite/*/lib/pkgconfig/ go run make.go
```
You can find more detail on https://github.com/mattn/go-sqlite3/issues/45.

## Run and initialize Camlistore
Now you can start the camlistore server by
```
./bin/camlistored
```
It will then generate server configuration file, at `$HOME/.config/camlistore/server-config.json`, and a new secret keyring: `$HOME/.config/camlistore/identify-secring.gpg`.
You should then see the following message after starting the camlistore server:
```
Generated new identity with keyId "1427D50D"
```
Use this key to initialize client command tools:
```
./bin/camput init --gpgkey 1427D50D
```
The client configuration file is located at
```
$HOME/.config/camlistore/client-config.json
```

I will write about the basic usage in next post in this series.
