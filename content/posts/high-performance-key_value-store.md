---
title: "High Performance Key_Value Store"
date: 2017-08-15
tags: ["Go", "talk"]
isCJKLanguage: true
---

{{< youtube ttebJcN5bgQ >}}

###   定义问题

满足条件的KV系统是什么样的，对当时现有的方案进行评估

###   自己干

Simplicity放在第一位，引用了两个talk，之后补上

-   https://go-talks.appspot.com/github.com/dgryski/talks/dotgo-2016/slices.slide###19
-   https://dave.cheney.net/2015/03/08/simplicity-and-collaboration

不需要打造一个通用的kv系统，而是满足特性需求的，根据关键点来简化设计

###   数据结构设计

基于最简单的interface设计，从单一的batch，到多个batch组成的栈，再到写入文件。

###   项目介绍

https://github.com/couchbase/moss

test比实际代码量更大

项目演进的循环：Implementation -> Benchmark -> Analyze and Profile

###   链接

https://speakerdeck.com/mschoch/value-store-in-go