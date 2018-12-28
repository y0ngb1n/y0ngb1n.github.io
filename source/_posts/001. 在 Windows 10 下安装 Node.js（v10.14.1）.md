---
title: 在 Windows 10 下安装 Node.js（v10.14.1）
cover: 'https://i.loli.net/2018/12/28/5c25bee4da8ca.png'
subtitle: 在 Windows 10 下安装 Node.js（v10.14.1），获取最新的 LTS 版本。
tags:
  - node.js
  - npm
  - cnpm
author:
  nick: y0ngb1n
  link: 'https://www.github.com/y0ngb1n'
abbrlink: b9f01718
date: 2018-12-07 22:00:00
---
## 准备工作

前往「[官方下载页面](https://nodejs.org/en/download/)」获取最新的 LTS 版本，当前为 `Latest LTS Version: 10.14.1`，官方提供了不同平台的安装文件，我们选择 `Windows Binary (.zip) 64-bit`，点击下载压缩版的二进制文件。

![图片](https://dn-coding-net-production-pp.codehub.cn/1b43306b-8613-409b-9800-900f3bc23e5f.png)

## 进入安装

下载完成后进行解压，为了方便管理，我新建了 `F:\Node.js\dev_tools\node\v10.14.1` 用来保存解压后 Node.js 的程序，并默认安装了 npm。

![图片](https://dn-coding-net-production-pp.codehub.cn/bdc50741-6ec3-4a6b-b773-fcba1de7367a.png)

为了在 CMD 中使用 Node.js 的相关命令，接下来添加「环境变量」：

```
# 新建 Node.js 安装路径的环境变量
NODE_HOME=F:\Node.js\dev_tools\node\v10.14.1

# 添加至 Path 下
Path={...};%NODE_HOME%;
```

> 由于 Node.js 中默认安装了 npm，所以不用额外配置就能在全局命令中使用 `npm` 命令，如果要使用自己安装的 npm 时，如 cnpm  ，那么就需要像上面一样添加相应的环境变量

## 测试

在 PowerShell 下输入 `node -v` 与 `npm -v`：

```console
PS C:\> node -v
v10.14.1
PS C:\> npm -v
6.4.1
```

可以看到当前 node 和 npm 的安装版本分别为：`v10.14.1`、`6.4.1`。

## NPM 配置

### 查看当前配置

使用 `npm config list` 当前配置，或使用 `npm config ls -l` 全部配置信息。

### 全局模块目录 及 缓存目录

配置 npm 安装的 `全局模块目录`，以及 `缓存目录`。

> 为什么要配置这两个目录呢？

在执行全局安装语句时，如：

```bash
npm install express -g
```

- `-g`：可选参数 -g，g 代表 global，全局安装的意思

当前是基于解压版安装的，默认会将 `express` 模块安装至 `{解压目录}\node_modules` 目录中，如我当前的是：`F:\Node.js\dev_tools\node\v10.14.1\node_modules`；npm 的缓存文件会保存至 `C:\Users\%USERNAME%\AppData\Roaming\npm-cache` 目录。如果是基于安装文件直接安装的，那么这两个文件夹都默认在 C 盘下，这样就会占用我们 C 盘的空间。

> 可以自定义指定这两个文件夹吗？

接下来开始配置这两个目录，指定「全局模块的安装目录」、「缓存目录」：

```bash
# 配置全局模块安装目录，文件会保存至 node_modules 文件夹
npm config set prefix "F:\Node.js\dev_tools\node\v10.14.1"

# 配置缓存目录
npm config set cache "F:\Node.js\dev_tools\node\v10.14.1\npm-cache"

# 配置后可通过下面方式来验证是否成功
npm config list
# 或
npm config ls -l
```

此时我们再执行一次全局安装 `express` 模块，可以看到出现了我们指定的目录。

> 我们的自定义配置会保存在 `C:\Users\%USERNAME%\.npmrc` 文件中。

### 配置 NPM 镜像源

我们可以指定 npm 的镜像源达到网络加速的效果，默认的源为：`https://registry.npmjs.org`，在国内访问速度较慢。

此时，我们就可以使用一些国内优秀的 npm 镜像源，如：

- [CNPM](https://cnpmjs.org/)：`https://r.cnpmjs.org/`
- [淘宝 NPM 镜像](https://npm.taobao.org/)：`https://registry.npm.taobao.org/`

> 临时使用

```bash
npm --registry https://registry.npm.taobao.org install express -g
```

> 持久使用

```bash
npm config set registry https://registry.npm.taobao.org

# 配置后可通过下面方式来验证是否成功
npm config get registry
# 或
npm info express
```

> 通过 `cnpm` 使用

```bash
npm install -g cnpm --registry=https://registry.npm.taobao.org

# 使用
cnpm install express -g

# 如果不能使用 cnpm，可能是指定了 npm 的全局模块目录导致，需要配置相应的系统环境，自行参考上面的「进入安装」模块
```

- 注意：此时的 cnpm 也会有默认的配置，同样需要进行「NPM 配置」模块那样进行 `全局模块目录` 及 `缓存目录` 的相关设置。自定义配置会保存在 `C:\Users\%USERNAME%\.cnpmrc` 文件中。

## 参考资料

- [安装 Node.js 和 npm](https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/00143450141843488beddae2a1044cab5acb5125baf0882000)
- [Node.js 安装配置](http://www.runoob.com/nodejs/nodejs-install-setup.html)
- [Node.js 安装和配置（windows 版）](https://www.jianshu.com/p/cc26e5d0f10f)
- [Node.js 安装及环境配置之 Windows 篇](https://www.jianshu.com/p/03a76b2e7e00)
- [在 Windows 环境下安装并配置 Node.js](https://www.jianshu.com/p/9f0481237691)
- [国内优秀 npm 镜像推荐及使用](http://riny.net/2014/cnpm/)

