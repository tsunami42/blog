---
title: "Bash Proxy Setting Function"
date: 2017-08-16
tags: ["Shell"]
isCJKLanguage: true
---

由于公司自带的代理失效，而telegraf的项目管理又用的是Godep，并没有vendor。导致telegraf本地编译失败，索性花了10分钟谢了几个帮助函数，一劳永逸。

git的代理是支持socks5的，可以直接设置。

shell通过环境变量设置只有`http_proxy`和`https_proxy`这两个变量，不过可以直接指向ss的本地代理端口

将一下函数加入.bashrc即可

```bash
function setproxy() {
    export {http,https,ftp}_proxy="http://127.0.0.1:1080"
}

# Unset Proxy
function unsetproxy() {
    unset {http,https,ftp}_proxy
}

# Set Git Proxy
function setgitproxy() {
    git config --global http.proxy 'socks5://127.0.0.1:1080'
    git config --global https.proxy 'socks5://127.0.0.1:1080'
}

# Unset Git Proxy
function unsetgitproxy() {
    git config --global --unset http.proxy
    git config --global --unset https.proxy
}
```