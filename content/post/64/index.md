---
title: "VScode编写C语言中文乱码问题"
slug: "64"
date: 2020-03-04T22:33:00+08:00
lastmod: 2020-03-04T22:33:00+08:00
categories: ["技术"]
tags: ["Windows", "vscode", "C"]
draft: false
---

## 说明

在VScode上写C语言，总是出现中文乱码问题，找了好多方法都不管用，最后发现了一个方法解决了问题，特此记录。

### 步骤

1. 打开Windows`控制面板`，点击`时钟和区域`，然后点击图中按钮

![更改日期、时间或数字格式](https://cdn.rumosky.com/usr/uploads/2020/03/1071478266.png)

2. 点击`管理`，`更改系统区域设置`

![更改系统区域设置](https://cdn.rumosky.com/usr/uploads/2020/03/361474971.png)

3. 勾选图中选项，然后点击确定，重启电脑

![使用utf8全球语言支持](https://cdn.rumosky.com/usr/uploads/2020/03/558196167.png)

#### 写在最后

很多博文上都让修改VScode的文件编码格式为GB2312，但不管用，而且，这个方法是全局修改，如果开发的其他项目是utf8编码，会有影响。