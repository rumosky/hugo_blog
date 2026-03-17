---
title: "Mindoc编译安装指南（Linux版）"
slug: "24"
date: 2019-03-12T20:35:00+08:00
lastmod: 2019-03-12T20:35:00+08:00
categories: ["技术"]
tags: ["linux", "mindoc"]
draft: false
---

## 说明

mindoc文档系统想做一些修改，就必须使用源码进行安装，在网上找了很多教程，都没有使用源代码进行安装，遂做一个记录。

地址：[mindoc](https://github.com/mindoc-org/mindoc)

本文用来介绍如何使用源代码来进行mindoc文档系统的搭建，若想了解已打包好的发行版安装办法，请参考：[bspost cid="12"]

### 编译步骤

环境搭建：服务器需要已经搭建好`go`语言环境，请参考：[bspost cid="22"]

同时需要安装`dep`依赖管理工具，请参考：[bspost cid="23"]

上述内容配置好之后，在gopath下的src目录里执行下面的命令：

```bash
git clone https://github.com/mindoc-org/mindoc
```

切换至mindoc目录，即执行

```bash
cd mindoc
```

执行

```bash
dep ensure
```

> 注意：这个`dep ensure`命令国内服务器需要自备梯子或者更换为国内镜像，否则会出错，国外服务器请忽略。

最后一步执行

```bash
go build
```

### 安装

编译完成之后，可以删除mindoc文件夹里的go源代码，剩余的文件保留即可，也可以什么都不管。 将mindoc文件夹打包，这个压缩包就相当于GitHub上的release版，后续安装请参考：[bspost cid="12"]

#### 注意

使用自己编译的mindoc系统安装时，教程前面的内容可以忽略，直接从下载程序后面开始。

再次强调，自己编译之后的文件相当于在GitHub上下载好的发行版。