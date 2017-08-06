---
title: "Hugo Notes"
date: 2017-08-05T14:02:05+08:00
draft: false
---

#   安装 & 初始化

https://gohugo.io/getting-started/installing

##  安装

Gopher当然是直接从源码安装了

```
go get github.com/kardianos/govendor
govendor get github.com/gohugoio/hugo
go install github.com/gohugoio/hugo
```

##  初始化

### 目录结构初始化

```shell
hugo new site blog
```

### 设置主题

```shell
cd blog;
git init;
git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke;

# 编辑config.toml配置文件，增加主题配置
echo 'theme = "ananke"' >> config.toml
```

文档上的默认主题是这个，考虑换一个

#   部署到github

[github page介绍](https://help.github.com/articles/user-organization-and-project-pages/)

##  新建项目

在github上新建项目，页面访问链接： `username.github.io/projectname`   

##  第一篇文章

```shell
# 新建模板文件
hugo new posts/my-first-post.md

# 随便写点啥

# 本地浏览器预览
hugo server -D
```

到目前为止，页面已经可以预览，接下来是发布工作

##  部署实践

### 准备工作

首先修改 `config.toml` 中的 `baseURL` 字段为
```
baseURL = "https://username.github.io/projectname"
```

然后是将 `posts/my-first-post.md` 中开头的 `draft: true` 改成 `false` 

### 推送

github pages的部署方式有好几种，我选择将生成的静态文件放到 `gh-pages` 目录中

[查看文档](https://gohugo.io/hosting-and-deployment/hosting-on-github/#deployment-from-your-gh-pages-branch)

### 注意事项

-   因为github是全站https开启的， `baseURL` 一定要是 `https` 开头，不然会有资源加载的问题
-   运行 `hugo` 命令会生成静态文件，默认生成的静态文件会放到 `public` 目录下

##  自动刷新的坑

修改页面，推送到`gh-pages` 分支上去之后，等了半天页面不会自动刷新，搜了一下，是时区问题。

根据jekyll的配置，在public目录下增加 `_config.yml`文件，指定时区就完美解决。

```shell
$ cat public/_config.yml
timezone: Asia/Shanghai
```

相关链接：https://stackoverflow.com/questions/20422279/github-pages-are-not-updating