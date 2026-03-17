---
title: "AMD处理器无法安装Android Studio虚拟机解决办法"
slug: "47"
date: 2019-10-17T00:58:00+08:00
lastmod: 2019-10-17T00:58:00+08:00
categories: ["技术"]
tags: ["Windows", "android", "AMD"]
draft: false
---

## 说明

电脑的CPU是锐龙`R5 1600X`，开发`Android`的时候发现无法安装`AVD`（安卓虚拟机），找了许久，终于解决。

### 步骤

运行`Android Studio`安装`AVD`虚拟机的时候出现下图错误：

![AVD错误](https://cdn.rumosky.com/usr/uploads/2019/10/1315529105.png)

此错误说明你的CPU没有开启`VT-x`或`SVN`虚拟化功能，若已经确认开始虚拟化功能，则说明此虚拟机不支持AMD处理器

#### 下载安装

`genymotion`官网：[genymotion](https://www.genymotion.com "genymotion")

注册下载`genymotion`虚拟机（此虚拟机支持AMD处理器）

> 若电脑没有安装`Oracle VM VirtualBox`，请选择附加`Oracle VM VirtualBox`的安装包

若不想注册，请使用下列下载地址直接下载：

```bash
# 纯genymotion安装包（不包含VirtualBox）
https://dl.genymotion.com/releases/genymotion-3.0.3/genymotion-3.0.3.exe

# genymotion安装包（包含VirtualBox）
https://dl.genymotion.com/releases/genymotion-3.0.3/genymotion-3.0.3-vbox.exe

# 截至本文发布，最新版为3.0.3
```

#### 运行

1.打开`genymotion`，选择需要的手机类型，点击`install`，如图：

![genymotion虚拟机](https://cdn.rumosky.com/usr/uploads/2019/10/789433210.png)

2.安装好之后，点击`start`运行虚拟机，如图：

![genymotion虚拟机](https://cdn.rumosky.com/usr/uploads/2019/10/1771016337.png)

3.打开Android Studio项目，在界面右上角，AVD设备这里就可以看到刚刚运行的虚拟机，先选择相应的设备，再点击运行即可

![运行工程](https://cdn.rumosky.com/usr/uploads/2019/10/1410565321.png)

4.项目运行成功！

![运行成功](https://cdn.rumosky.com/usr/uploads/2019/10/1204678360.png)