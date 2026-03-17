---
title: "Go语言环境搭建（CentOS 7）"
slug: "22"
date: 2019-03-10T19:49:00+08:00
lastmod: 2019-03-10T19:49:00+08:00
categories: ["技术"]
tags: ["linux", "golang"]
draft: false
---

## GO语言

Go（又称Golang）是Google开发的一种静态强类型、编译型、并发型，并具有垃圾回收功能的编程语言。

地址：[Go](https://github.com/golang/go)

最近要用到go语言编写的一个开源项目，需要自己编译一下，所以做个笔记。

### 环境搭建

请依照步骤进行

#### 下载安装包

```bash
# 国内地址
wget https://studygolang.com/dl/golang/go1.12.linux-amd64.tar.gz

# 官方地址
wget https://dl.google.com/go/go1.12.linux-amd64.tar.gz
```

#### 解压安装

将上述安装包解压至/usr/local目录。

```bash
tar -C /usr/local -xzf go1.12.linux-amd64.tar.gz
```

#### 环境变量

使用下列命令切换至环境变量配置文件

```bash
vi /etc/profile
```

按`i`进入编辑模式，在文件底部最后一行添加下列内容：

```bash
export GOROOT=/usr/local/go
export GOPATH=/home/gopath
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
```

上述内容添加完毕之后，保存文件并退出，然后执行`source /etc/profile`命令刷新环境变量

##### 说明

`GOROOT`是go的根目录，也就是运行目录，相当于Java的`JDK`目录，让系统可以找到go指令 `GOPATH`是go的工作目录，在这个目录下，新建一个`src`目录，在`src`里面放置你的go源代码

### 验证安装

##### 使用`go`命令验证（推荐）

在任意目录下，输入`go`命令，出现下图界面则说明配置正确。

![go语言环境配置](https://cdn.rumosky.com/usr/uploads/2019/03/3762089968.png)

若出现`command not found`一般是环境变量没有配置好

##### 使用代码验证

在gopath/src目录之下，新建一个目录test，然后新建一个文件`hello.go`，内容如下：

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, world")
}
```

保存文件之后执行命令`go run hello.go`，输出`Hello, world`即证明配置正确

### 更新国内镜像源

这里补充一下，由于众所周知的原因，官方源比较慢，这里介绍如何添加国内源，地址：[goproxy](https://goproxy.cn/)

官网上有详细的配置命令，这里就不赘述了。