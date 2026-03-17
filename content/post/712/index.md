---
title: "Mac OS M2芯片第三方显示器无法调整声音"
slug: "712"
date: 2023-10-19T23:05:11+08:00
lastmod: 2023-10-19T23:05:11+08:00
categories: ["随笔"]
tags: ["4K显示器", "Apple", "苹果", "Mac"]
draft: false
---

## 前言

Mac mini搭配华为MateView 28寸显示器，系统自带的OSD菜单无法调节音量和亮度，记录一下

### 步骤

经过排查，M系列芯片的苹果电脑，跟很多显示器都不兼容，无法调节音量和亮度，只有苹果自己的studio display可以完美兼容，intel芯片的苹果电脑是没问题的。

软件地址：[MonitorControl](https://github.com/MonitorControl/MonitorControl)

这个软件是国外一个大佬写的，可以兼容很多显示器，安装之后顶部菜单会有一个太阳图标，点击之后可以显示亮度和音量调节，如下图：

![1697727777010.jpg](https://cdn.rumosky.com/usr/uploads/2023/10/1404865632.jpg)

这个软件还可以默认屏蔽系统自带的OSD调节菜单，更多设置内容请自行探索