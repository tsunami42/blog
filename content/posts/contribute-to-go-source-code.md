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

关于commit message

https://go-review.googlesource.com/c/49090

https://go.googlesource.com/go/+/cc1f58046f79698c243b4735c06e4e08263f5dc5

原来的message
```
fmt: Example showing implementation of Stringer

There was no existing examples showing a custom implementation of the
Stringer interface.

Change-Id: I901f995f8aedee47c48252745816e53192d4b7e4
```

修改意见：https://go-review.googlesource.com/c/49090/1//COMMIT_MSG#7
```
The first line of the commit message should explain what will occur when this change is merged. eg:

fmt: add Stringer example
```

要加上完整的包名才行

https://go-review.googlesource.com/c/53357/1//COMMIT_MSG#7

这样在gerrit的页面上显示的时候，在会有完整的信息