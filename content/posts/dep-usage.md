---
title: "Dep使用记录"
date: 2017-08-04T23:35:01+08:00
tags: ["Go", "dep"]
draft: false
---

#   dep介绍

今年GopherCon刚好有两个关于dep的talk

-   [The New Era of Go Package Management](https://www.youtube.com/watch?v=5LtMb090AZI&list=PL2ntRZ1ySWBdD9bru6IR-_WXUgJqvrtx9&index=18)
-   [go dep in 10 minutes](https://www.youtube.com/watch?v=eZwR8qr2BfI&list=PL2ntRZ1ySWBfhRZj3BDOrKdHzoafHsKHU&index=16)

# 使用方法

```shell
$ dep init
```

初始化项目，会先从已有的包管理读取配置依赖信息（`glide` & `godep`），备份当前的`vendor/`然后生成相应的文件 [`Gopkg.toml`](https://github.com/golang/dep/blob/master/docs/Gopkg.toml.md) 与`Gopkg.lock`，最后将依赖安装到`vendor/`目录下

```shell
$ dep ensure
```

保证`vendor/`目录下的依赖符合`Gopkg.toml`与`Gopkg.lock`中的配置，如果没有就创建目录

#   使用时的坑

我的项目有两个依赖：  

-   `github.com/influxdata/influxdb@v1.0.0`    
-   `github.com/Shopify/sarama@v1.12.0`   

原先是通过gdm来直接保存和修改GOPATH中包的版本信息，没有使用vendor的功能，所有的信息都直接从src目录下代码的版本号获得

在执行`dep init`后， `Gopkg.toml`里influxdb的依赖一直不对，一直是当前最大的tag，v1.3.1，多次尝试后，通过`dep help init`找到了答案

```
$ dep help init
...
By default, the dependencies are resolved over the network. A version will be
selected from the versions available from the upstream source per the following
algorithm:
 - Tags conforming to semver (sorted by semver rules)
 - Default branch(es) (sorted lexicographically)
 - Non-semver tags (sorted lexicographically)
An alternate mode can be activated by passing -gopath. In this mode, the version
of each dependency will reflect the current state of the GOPATH. If a dependency
doesn't exist in the GOPATH, a version will be selected based on the above
network version selection algorithm.
...
```

默认情况下，`dep init`是直接从网上获取版本信息，只有指定了`-gopath`，才是从当前的GOPATH中获取。