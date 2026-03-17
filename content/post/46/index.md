---
title: "Android SDK下载配置"
slug: "46"
date: 2019-10-17T00:56:00+08:00
lastmod: 2019-10-17T00:56:00+08:00
categories: ["技术"]
tags: ["Windows", "android"]
draft: false
---

## 说明

Android SDK有三种方式获取，特此记录。

### 使用`SDK Tools`下载

国内地址：[Android SDK](https://www.androiddevtools.cn/ "Android SDK")

进入网站后选择`SDK Tools`，如图：

![SDK Tools](https://cdn.rumosky.com/usr/uploads/2019/10/1532879675.png)

选择对应的系统下载安装，然后在软件中配置SDK路径，详见本文最后一张图。

### 直接下载`SDK`

此方式是直接在国内地址下载完整的`SDK`文件，下载好之后解压到`Android Studio`的`SDK`目录下即可，但此方式不推荐，因为更新很不即时，且手动安装`SDK`会出现很多错误。如图：

![SDK](https://cdn.rumosky.com/usr/uploads/2019/10/1435328918.png)

### `Android Studio`配置国内镜像源

`Android Studio`本身就可以下载SDK，只是访问的是国外的`Android`官网，国内无法访问，所以配置国内`SDK`地址就可以，参考：[bspost cid="45"]

配置好之后可以直接在下图页面中下载`SDK`，如果你可以科学上网，不需要配置国内镜像即可在此处下载`SDK`

![SDK配置](https://cdn.rumosky.com/usr/uploads/2019/10/1372397314.png)