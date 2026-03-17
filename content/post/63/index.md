---
title: "VScode运行C程序无法输入值"
slug: "63"
date: 2020-03-04T22:19:00+08:00
lastmod: 2020-03-04T22:19:00+08:00
categories: ["技术"]
tags: ["Windows", "vscode", "C"]
draft: false
---

## 说明

如题，由于VScode输出是只读状态，所以当程序里有`scanf`这类函数时，程序就无法继续运行，只能重启VScode或者在任务管理器里关闭程序。

### 步骤

1.点击运行之后，输出页面无法输入值，再次点击运行，则显示程序已经在运行。

![输出页面无法输入](https://cdn.rumosky.com/usr/uploads/2020/03/1651392959-1.png)

2.打开`文件 -&gt; 首选项 -&gt; 设置`，在上方的搜索框里输入`run in terminal`，勾选此项设置

![更改设置](https://cdn.rumosky.com/usr/uploads/2020/03/1665448849.png)

3.再次运行程序，在终端界面输入值，完成！

![运行程序](https://cdn.rumosky.com/usr/uploads/2020/03/51577930.png)