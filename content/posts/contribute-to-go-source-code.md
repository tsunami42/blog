---
title: "Contribute To Go Source Code"
date: 2017-08-08
tags: ["Go", "Tutorial"]
draft: true
---

{{< youtube DjZMKKfNVMc >}}

切换到old ui，选择agreement 
Google Contributor License Agreement

git change bits_reverse_bytes

doc/root.html

template: main:6:10: executing "main" at <$.GoogleCN>: can't evaluate field GoogleCN in type godoc.Page 

删除 go-tools 的依赖

go get golang.org/x/tools/cmd/godoc

godoc -goroot=$HOME/go -http=:6060 -ex

https://go-review.googlesource.com/54670