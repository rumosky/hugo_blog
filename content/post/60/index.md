---
title: "IntelliJ IDEA中maven依赖无法下载的解决办法"
slug: "60"
date: 2020-02-22T22:05:00+08:00
lastmod: 2020-02-22T22:05:00+08:00
categories: ["技术"]
tags: ["Windows", "IDEA", "maven"]
draft: false
---

## 说明

如题，解决办法有三种，请自行尝试。

### 步骤

请按照步骤进行

#### 方法1

对下图中的两个位置点击刷新按钮

![刷新maven依赖](https://cdn.rumosky.com/usr/uploads/2020/02/234416721.png)

#### 方法2

勾选下图中红框的部分；该菜单路径为：`File -> Build, Execution, Deployment -> Build Tools -> Maven -> Importing`

![自动导入maven](https://cdn.rumosky.com/usr/uploads/2020/02/647273132.png)

#### 方法3

配置国内maven阿里云镜像，参考：[bspost cid="59"]

### 写在最后

一般情况下，这三种方法总能解决maven依赖问题，若还是不行，请自行百度。