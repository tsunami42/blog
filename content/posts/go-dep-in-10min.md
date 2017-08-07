---
title: "Go Dep In 10min"
date: 2017-08-07T22:17:26+08:00
tags: ["talk", "Go"]
---

{{< youtube eZwR8qr2BfI >}}

### Manifest - Gopkg.toml

用户自行维护
```toml
[[constraint]]
    name = "github.com/sdboyer/deptest"
    source = "http://github.com/carolynvs/deptest"
    version = "0.8.0"

[[constraint]]
    branch = "master"
    name = "github.com/pkg/errors"
```

根据semver指定版本

### Lock - Gopkg.lock
```toml
[[projects]]
  name = "github.com/sdboyer/deptest"
  revision = "ff2948a2ac8f538c4ecd55962e919d1e13e74baf"
  version = "v0.8.1"

[[projects]]
  name = "github.com/pkg/errors"
  revision = "17b591df37844cde689f4d5813e5cea0927d8dd2"
  version = "v0.7.1"
```

### 0.8.0 和 v0.8.1 ？

dep希望大家有使用semver的规范来管理release，默认指代一个版本范围
```
1.2.3 becomes >=1.2.3, <2.0.0
0.2.3 becomes >=0.2.3, <0.3.0
0.0.3 becomes >=0.0.3, <0.1.0
 
Use =0.8.1 to pin to a version
```

### 一些命令

#### dep init

-   使用其他的包管理：会想办法自动迁移
-   GOPATH就是当前的依赖：dep init -gopath
-   已经用了vendoer：先备份，在根据vendor更新

#### dep ensure
初始化 vendor & 在manifest修改之后

#### dep status

```
PROJECT                             CONSTRAINT     VERSION        REVISION  LATEST
github.com/Masterminds/semver       branch 2.x     branch 2.x     139cc09   c2e7f6c
github.com/Masterminds/vcs          ^1.11.0        v1.11.1        3084677   3084677
github.com/armon/go-radix           *              branch master  4239b77   4239b77
```

### 依赖修改

-   增加依赖：代码里增加import，dep ensure
-   删除依赖：删除import的代码，dep ensure，手动从manifest中删除
-   测试还没提交的依赖修改：从vendor中移除，不要执行dep ensure，在GOPATH中修改依赖

### Floor is Lava

配置文件的格式不会变，dep init可以用，PR里有大新闻

### 相关链接

https://github.com/golang/dep
https://github.com/Masterminds/semver
http://carolynvs.com/dep-in-10