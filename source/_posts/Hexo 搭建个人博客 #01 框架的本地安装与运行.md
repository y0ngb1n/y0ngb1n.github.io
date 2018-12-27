---
title: 'Hexo 搭建个人博客 #01 框架的本地安装与运行'
cover: 'https://i.loli.net/2018/12/27/5c248693e4080.jpg'
subtitle: 'Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。'
categories:
  - Hexo 搭建个人博客
tags:
  - hexo
author:
  nick: y0ngb1n
  link: 'https://www.github.com/y0ngb1n'
abbrlink: 368bdd3e
date: 2018-12-09 00:00:00
---
## 前言

进入「[Hexo 官网](https://hexo.io/)」便可以看到一句醒目的 Slogan：

> A fast, simple & powerful blog framework
> 快速、简洁且高效的博客框架

Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

## 安装

由于 Hexo 需要基于 [Node.js](https://nodejs.org/) 与 [Git](https://git-scm.com/) 进行框架的搭建（请自行 Google Node.js、Git 的安装），下面开始正式安装。

```console
# 安装 Hexo 提供的脚手架 hexo-cli
$ npm install hexo-cli -g
```

## 运行

接着进行初始化项目，以及本地运行 Hexo。

```console
# 初始化博客，会在本地生成一个 blog 的文件夹，用于存放工程源码，blog 可以任意
$ hexo init blog
$ cd blog

# 安装项目所需要的依赖
$ npm install

# 启动博客
$ hexo server
INFO  Start processing
INFO  Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.
```

此时在浏览器中打开「[http://localhost:4000](http://localhost:4000)」即可看到 Hexo 默认主题的博客站点。

![Hexo 默认主题](https://dn-coding-net-production-pp.codehub.cn/f988803e-dc43-4ae5-8c1e-b65cb8052244.png)

## 常用命令

```console
$ hexo n "myblog"   # => hexo new "myblog"
$ hexo p            # => hexo publish
$ hexo g            # => hexo generate
$ hexo s            # => hexo server
$ hexo d            # => hexo deploy
```

> 关于 Hexo 的服务器命令

```console
$ hexo server                   # Hexo 会监视文件变动并自动更新，无须重启服务器
$ hexo server -s                # 静态模式
$ hexo server -p 5000           # 更改端口
$ hexo server -i 192.168.1.1    # 自定义 IP
$ hexo clean                    # 清除缓存，网页正常情况下可以忽略此条命令
$ hexo g                        # 生成静态网页
$ hexo d                        # 开始部署
```

## 参考资料

- [什么是 Hexo？](https://hexo.io/zh-cn/docs/)
- [Hexo 指令](https://hexo.io/zh-cn/docs/commands.html)
- [Hexo 常用命令笔记](https://www.jianshu.com/p/83d5989bd496)

