---
title: "oneindex搭建网盘"
slug: "25"
date: 2019-03-13T20:39:00+08:00
lastmod: 2019-03-13T20:39:00+08:00
categories: ["技术"]
tags: ["olaindex", "onedrive"]
draft: false
---

## 介绍

本文介绍oneindex程序使用方法。

源码请自行在GitHub上寻找，搜索oneindex即可

### 步骤

1.准备好一个onedrive账号，自己购买商业版或者使用免费教育版。教育版onedrive注册教程百度有很多，这里就不赘述了。

2.在网站目录执行命令`git clone https://github.com/donwa/oneindex.git`

3.使用宝塔面板新建一个网站，根目录选择刚刚下载的`oneindex`目录，填写域名，不需要数据库。

4.修改`oneindex`目录下的`config`和`cache`目录权限为`777`

5.在浏览器输入域名，开始安装

6.安装流程如下图

![oneindex_install](https://cdn.rumosky.com/usr/uploads/2023/04/2631640726.gif)

到此就安装成功了，oneindex程序会将你绑定的onedrive账号目录下的文件全部列出，可以实现直连下载，速度还可以。

#### 注意

国内教育邮箱不一定有足够的权限，建议使用国外教育邮箱