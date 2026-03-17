---
title: "爱之初体验：phpstudy-linux面板(小皮面板)首次使用感受"
slug: "52"
date: 2019-10-25T01:08:00+08:00
lastmod: 2019-10-25T01:08:00+08:00
categories: ["随笔"]
tags: ["linux", "宝塔面板", "Windows"]
draft: false
---

## 说明

前一阵子看到phpstudy发布了Linux面板，类似于宝塔，遂想体验一番。

### 介绍

phpstudy-linux面板(小皮面板)官网：[phpstudy](https://www.xp.cn/linux.html "phpstudy")

主要功能

![主要功能](https://cdn.rumosky.com/usr/uploads/2019/10/4023865038.png)

#### 安装

```bash
# Centos安装脚本 
yum install -y wget && wget -O install.sh https://download.xp.cn/install.sh && sh install.sh

# Ubuntu安装脚本 
wget -O install.sh https://download.xp.cn/install.sh && sudo bash install.sh

# Deepin安装脚本 
wget -O install.sh https://download.xp.cn/install.sh && sudo bash install.sh

# Debian安装脚本 
wget -O install.sh https://download.xp.cn/install.sh && sudo bash install.sh
```

安装完成如下：

![安装完成](https://cdn.rumosky.com/usr/uploads/2019/10/30212776.png)

使用`xp`命令可以获取以下提示：

![xp命令](https://cdn.rumosky.com/usr/uploads/2019/10/1426023078.png)]

#### 界面

登陆界面

![登陆界面](https://cdn.rumosky.com/usr/uploads/2019/10/1160863277.png)

默认主页面

![默认主页面](https://cdn.rumosky.com/usr/uploads/2019/10/2889665982.png)

监控页面

![监控页面](https://cdn.rumosky.com/usr/uploads/2019/10/2827360119.png)

默认新建站点首页

![默认新建站点首页](https://cdn.rumosky.com/usr/uploads/2019/10/609477425.png)

## 与宝塔面板对比

宝塔面板使用了一年多，从5.x到现在的7.0，宝塔确实越做越好，正好来对比以下：

> 以下仅是个人使用感受，不喜勿喷，仅供参考

1. 宝塔软件管理相对更好，例如，nginx直接升级就行，不像小皮是直接全部的版本在列表里展开，况且好像没有最新版，也不是非常全。
2. 宝塔文件管理可以批量复制移动文件，但是小皮不行，这让我测试时解压缩文件后不方便移动文件。
3. SSL证书不能一键免费申请部署。
4. 没有与phpstudy账号绑定，也没有小程序可以监控。
5. 防火墙做的有些简陋，功能并不齐全。
6. BUG有点多，比如我新建的typecho程序用Nginx就无法访问，但是用Apache就没问题。
7. 群里猪哥回复是很及时，但是没有解决我的问题，并且后续再没理我。


## 总结

**划重点：小皮免费！！！**

宝塔确实好，但是宝塔很多功能是要付费的，一分钱一分货，理解。小皮面板完全免费可以做到这种程度已经很不错了，而且，小皮面板刚刚发布没多久，宝塔已经迭代很多版本，所以上述对比是很不公平的。

> 对服务器没什么大要求的小白客户其实可以考虑小皮，功能足够，还免费。作为宝塔用户我是懒得换了，而且，现在也不值得换。

最后，期待phpstudy做得更好！点赞！