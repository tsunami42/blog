---
title: "Hugo Notes 2"
date: 2017-08-07
tags: ["note"]
draft: true
---

###   静态资源加载失败

由于不是在网站的根目录部署页面，所以会导致大部分主题的css文件加载不出来，苦恼了半天，在命令行里找到了线索。   
修改config.toml，增加一项配置就可以解决
```toml
canonifyURLs = true
```
配置的作用：所有生成静态文件里的资源链接全部使用绝对链接。   
在自行部署，需要同时支持http和https时会有些问题，在github pages的场景下，这点小缺陷忽略不计

相关链接：https://discourse.gohugo.io/t/canonifyurls-relativeurls-interaction/5448

### 代码高亮

https://gohugo.io/tools/syntax-highlighting/

默认状态下，hugo不带高亮。如果服务端渲染，可以通过`Pygments`来实现。   
其余就是前端方案，我图省事，直接找到一个自带高亮的主题

### Markdown配置

https://gohugo.io/content-management/formats/

hugo使用的是`blackfriday`引擎，我自己加上了以下配置
```toml
[blackfriday]
    #   在新页面中打开链接
    hrefTargetBlank = true
    #   直接换行
    extensions = ["hardLineBreak"]
```

### 修改post的draft状态

开始我一直是手动修改draft的状态的，可以直接用命令行，会更新draft的状态，并且修改时间到当前时间
```shell
hugo undraft path/to/content
```
