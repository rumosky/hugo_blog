---
title: "dep：Go依赖管理工具安装指南"
slug: "23"
date: 2019-03-10T20:32:00+08:00
lastmod: 2019-03-10T20:32:00+08:00
categories: ["技术"]
tags: ["linux", "golang", "dep"]
draft: false
---

## 介绍

dep是Go的依赖管理工具。它需要Go 1.9或更新版本才能编译。

使用dep可以快捷的拉取go项目的一些依赖包。

地址：[dep](https://github.com/golang/dep)

使用的前请确保已经正确配置好了go语言环境，如未安装go，请参考：[bspost cid="22"]

## 安装

### 方法1：

`Ubuntu`和`Debian`可以使用`sudo apt-get install go-dep`命令来快捷安装，`CentOS`不适用。

### 方法2：

使用官方一键脚本安装

```bash
curl https://raw.githubusercontent.com/golang/dep/master/install.sh | sh
```

### 方法3：

使用go命令安装（推荐）

```bash
go get -u github.com/golang/dep/cmd/dep
```

#### 注意

以上三种方法，dep安装成功之后默认路径都在go语言环境的`gopath`目录之下的`bin`文件里，请添加`gopath`目录下`bin`文件为系统环境变量，否则会出现`command not found`，环境变量配置请参考本文开头引用的go环境配置一文。

## 验证

在任意目录下执行命令`dep`，出现下图结果则安装正常，否则请参考`注意`部分的内容。

![dep](https://cdn.rumosky.com/usr/uploads/2019/03/2562315012.png)