---
title: "win11 启动 Local Session Manager CPU占用100%"
slug: "848"
date: 2025-09-07T09:21:00+08:00
lastmod: 2025-10-16T22:37:00+08:00
categories: ["杂谈"]
tags: ["Windows", "win11"]
draft: false
---

## 说明

如题，这几天打开电脑出现了风扇狂转的情况，查看了一下发现是进程占用很高，找到了解决办法。


<!--more-->


## 步骤

在命令提示符(管理员)下键入以下命令：

```bash
sfc /SCANNOW

Dism /Online /Cleanup-Image /ScanHealth
```

这条命令将扫描全部系统文件并和官方系统文件对比，扫描计算机中的不一致情况。

```bash
Dism /Online /Cleanup-Image /CheckHealth
```

这条命令必须在前一条命令执行完以后，发现系统文件有损坏时使用。

```bash
DISM /Online /Cleanup-image /RestoreHealth
```

这条命令是把那些不同的系统文件还原成官方系统源文件。

完成后重启，再键入以下命令：

```bash
sfc /SCANNOW，
```

检查系统文件是否被修复。

同时检查更新您计算机所有的设备驱动程序.

## 补充

上述教程是微软官方给的回复，但我发现最直接的效果还是直接设置两个服务为手动就好了

```bash
Certificate Propagation
Smart Card
```