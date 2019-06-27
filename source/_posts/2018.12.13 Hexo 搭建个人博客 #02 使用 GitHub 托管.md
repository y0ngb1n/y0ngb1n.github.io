---
title: 'Hexo 搭建个人博客 #02 使用 GitHub 托管'
cover: 'https://i.loli.net/2018/12/27/5c248693e4080.jpg'
subtitle: 可以使用 GitHub Pages 免费托管我们的博客站点，并提供 username.github.io 二级域名、HTTPS 加持。
categories:
  - Hexo 搭建个人博客
tags:
  - hexo
  - github-pages
  - 个人博客
author:
  nick: y0ngb1n
  link: 'https://www.github.com/y0ngb1n'
abbrlink: bc7483d9
date: 2018-12-13 22:00:00
---

## 新建仓库

![新建仓库](https://dn-coding-net-production-pp.codehub.cn/15d87aee-b664-4463-bd1b-9a83d1989a85.png)

新建一个名为 `y0ngb1n.github.io` 的仓库，格式为：`<username>.github.io`。

GitHub 为我们提供了一个二级域名 `<username>.github.io`，如果我们为自己的某个项目开启了 [GitHub Pages](https://pages.github.com/) 功能后，我们可以使用 `http(s)://<username>.github.io/<projectname>` 进行访问，这样我们可以为每个项目添加上项目展示的 Pages；当我们「新建一个名为 `<username>.github.io` —自己对应的二级域名—的仓库」时，GitHub 会将该仓库路由为我们的个人主页，可使用 `http(s)://<username>.github.io` 进行访问。

## 提交源码

按照仓库页面上的提示进行操作即可：

```bash
# 初始化 git 仓库
git init

# 进行版本控制
git add .

# 提交
git commit -m ":tada: init hexo"

# 添加远程仓库（GitHub）地址
git remote add origin https://github.com/y0ngb1n/y0ngb1n.github.io.git

# 推送代码
git push -u origin master
```

过程会提示输入帐号密码，推送成功后，刷新仓库页面后即可看到我们提交的源码了。

![仓库主页](https://dn-coding-net-production-pp.codehub.cn/795b7e5f-5d4b-47be-aec1-351db7ee7c11.png)

## 开启 GitHub Pages 功能

先使用 Hexo 进行编译：

```bash
# 清除缓存，可选
hexo clean

# 编译
hexo g
```

然后使用 `hexo-deployer-git` 插件进行部署：

```bash
# 安装插件
npm install hexo-deployer-git
```

修改 `_config.yml`：

```yml
deploy:
  type: git
  repo: git@github.com:y0ngb1n/y0ngb1n.github.io.git
  branch: gh-pages
```

这里使用 `SSH` 协议，因为只能走 Git 协议，走 HTTPS 协议会报错；关于 `SSH` 的请参考「[Connecting to GitHub with SSH](https://help.github.com/articles/connecting-to-github-with-ssh/)」、「[SSH 原理与运用（一）：远程登录](http://www.ruanyifeng.com/blog/2011/12/ssh_remote_login.html)」。

然后进行部署：

```bash
hexo d
```

部署成功后，会自动在远程仓库创建 `gh-pages` 分支。

![图片](https://dn-coding-net-production-pp.codehub.cn/c0abda74-4a10-4185-8c89-9cf1825b0cba.png)

## 404 部署出问题了

原本是想部署 `gh-pages` 分支的，结果只能部署 `master` 分支，由于我们的 `master` 分支上没有 `index.html` 文件，所以访问主页时出现了 404；

经验证：是由于我们的仓库使用了二级域名的方式，GitHub Pages 只能部署 `master` 分支，且不能选择，如果把名字改为别的（如 `blog`），此时就能选择部署哪个分支了。

我想到的有两个解决方案：

1. 调整分支策略：
    - `master`：编译后的静态资源
    - `source`：博客源码
2. 修改仓库名（如 `blog`）：
    - 优点：可调整 github-pages 部署策略
    - 缺点：不能直接使用二级域名访问了，需要加上 `/blog` 后缀进行访问（可以自定义域名解决）

这里我选择方案 1（调整分支策略）：

```console
# 查看所有分支
$ git branch -a
  ls
* master
  remotes/origin/master

# *先在仓库设置中将 gh-pages 设置为默认分支（否则无法删除 master 分支）

# 重命名本地 master 分支为 source
$ git branch -m master source

# 删除远程 master 分支
$ git push --delete origin master
To github.com:y0ngb1n/y0ngb1n.github.io.git
 - [deleted]         master

# 上传新的 source 分支
$ git push origin source
To github.com:y0ngb1n/y0ngb1n.github.io.git
 * [new branch]      source -> source
```

然后修改 `_config.yml`，将编译后的静态部署至 `master` 分支：

```yml
deploy:
  type: git
  repo: git@github.com:y0ngb1n/y0ngb1n.github.io.git
  branch: master
```

修改完成后，执行 `hexo d` 进行编译部署，直到远程仓库出现新的 `master` 分支，命令如下：

```console
$ hexo d
INFO  Deploying: git
INFO  Clearing .deploy_git folder...
INFO  Copying files from public folder...
INFO  Copying files from extend dirs...
On branch master
nothing to commit, working tree clean
remote:
remote: Create a pull request for 'master' on GitHub by visiting:
remote:      https://github.com/y0ngb1n/y0ngb1n.github.io/pull/new/master
remote:
Branch 'master' set up to track remote branch 'master' from 'git@github.com:y0ngb1n/y0ngb1n.github.io.git'.
To github.com:y0ngb1n/y0ngb1n.github.io.git
 * [new branch]      HEAD -> master
INFO  Deploy done: git
```

此时 `github-pages` 监测到 `master` 分支有更新，会更新部署，此时访问 [https://y0ngb1n.github.io/](https://y0ngb1n.github.io/) 终于看到了期待以久的 Hexo 博客页面。

![y0ngb1n.github.io](https://dn-coding-net-production-pp.codehub.cn/d2760022-b64f-46e1-90bf-c74a5366631f.png)

因为我们调整了分支策略，所以此时 `source` 分支才是我们的主分支了，在设置中把 `source` 设置为默认分支，然后再把 `gh-pages` 分支从远程仓库中删除掉。

```console
# 删除远程 gh-pages 分支
$ git push --delete origin gh-pages
To github.com:y0ngb1n/y0ngb1n.github.io.git
 - [deleted]         gh-pages
```

至此就大功告成啦！

> 只有 GitHub 可能免费托管吗？

当然不止啦，其它平台也有提供这样的 Pages 功能，如：

- [GitLab Pages](https://docs.gitlab.com/ee/user/project/pages/)
- [码云 Gitee Pages Pro](https://gitee.com/help/articles/4228)
- [Coding Pages](https://coding.net/pages/) 

## 参考资料

- [GitHub Pages Basics](https://help.github.com/categories/github-pages-basics/)
- [Customizing GitHub Pages](https://help.github.com/categories/customizing-github-pages/)
- [单个 GitHub 帐号下添加多个 GitHub Pages 的相关问题](http://chitanda.me/2015/11/03/multiple-git-pages-in-one-github-account/)
- [Configuring a publishing source for GitHub Pages](https://help.github.com/articles/configuring-a-publishing-source-for-github-pages/)
- [Github Pages 部署个人博客 - Hexo 篇](https://sisibeloved.github.io/hexoBlog/2018/04/12/Github-Pages%E9%83%A8%E7%BD%B2%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2-Hexo%E7%AF%87/)
- [Connecting to GitHub with SSH](https://help.github.com/articles/connecting-to-github-with-ssh/)
- [SSH 原理与运用（一）：远程登录](http://www.ruanyifeng.com/blog/2011/12/ssh_remote_login.html)
- [Git 修改分支名称](https://www.jianshu.com/p/cc740394faf5)
- [Git 大全](https://gitee.com/all-about-git)

