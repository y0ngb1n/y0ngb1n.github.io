---
title: 'Hexo 搭建个人博客 #03 框架的基本结构与打包后的文件介绍'
cover: 'https://i.loli.net/2018/12/27/5c248693e4080.jpg'
subtitle: Hexo 框架的基本结构与打包后的文件介绍。
categories:
  - Hexo 搭建个人博客
tags:
  - hexo
  - 个人博客
author:
  nick: y0ngb1n
  link: 'https://www.github.com/y0ngb1n'
abbrlink: b821c4ca
date: 2018-12-15 22:00:00
---
## 目录结构

```console
.
├── .deploy_git
├── node_modules
├── public
├── scaffolds
│   ├── draft.md
│   ├── page.md
│   └── post.md
├── source
│   └── _posts
│       └── hello-world.md
├── themes
├── .gitignore
├── _config.yml
├── db.json
├── package.json
└── package-lock.json
```

### .deploy_git

在输入 `hexo d` 部署到 GitHub 后自动创建。该目录的结构和 `public` 目录基本一致（不一致的情况是由于重新生成，但是没有发布站点造成，此时 `public` 内容新于 `.deploy_git` 内容）。

### node_modules

存放安装的 Hexo 扩展，都是相应的 node 依赖模块。

### public

在执行 `hexo g` 命令时，Hexo 程序会编译 `source`、`theme` 目录，生成的静态网页内容目录就是 `piblic`。

生成好的 `public` 文件夹内容就可以直接当成静态网站进行部署。

在执行 `hexo d` 命令时，会将 `piblic` 目录内容复制到 `.deploy_git` 目录。

### scaffolds

`scaffolds` 是“脚手架、骨架”的意思，当你新建一篇文章（`hexo new 'title'`）的时候，Hexo 是根据这个目录下的文件进行构建的。

### source

存放用户资源的地方。

#### _posts

存放博客文章的地方，其中的 `markdown` 文件、HTML 文件、org 文件等会被解析并放到 `public` 文件夹，发布到站点。

#### 其它以 _（下划线）开头的文件 / 文件夹

将会被忽略。因此可以在 `source` 目录下创建  `_drafts` 目录用于存放未完成的草稿，其中内容不会发布到网站。

#### 其它非 _ 开头的文件 / 文件夹

会被拷贝到 `public` 目录并上传到站点。

- 可以创建 `img` 目录来存放在博客引用到的图片等。
- 要添加新的页面（例如 `about`），执行 `hexo new page PageName` 命令即会在 `source` 中自动新建子目录 `PageName` 。
- `favicon.ico` 在主题配置文件中引用 `/favicon.ico` 来设置站点的页面图标。

### themes

网站的主题目录。默认安装 `landscape` 主题，你可以安装新主题到 `themes` 目录，也可以自己新建主题。

### _config.yml

全局配置文件，网站的很多信息都在这里配置，诸如网站名称，副标题，描述，作者，语言，主题，部署等等参数。

### db.json

自动生成的一个文件 JSON 文件，可以对博客的文件进行管理查找的，可以忽略。

### package.json

Hexo 框架的参数和所有依赖的插件。

## 参考资料

- [Hexo 的目录结构](https://www.jianshu.com/p/17d55d420d94)
- [Hexo 的目录结构及作用](http://syxiaqj.github.io/2014/02/18/structure-of-hexo/)
- [Hexo 目录结构小探](https://yuchen-lea.github.io/2016-01-18-hexo-dir-struct/)

