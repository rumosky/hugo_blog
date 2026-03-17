---
title: "git bash使用atom-one-dark配色方案"
slug: "65"
date: 2020-03-16T22:34:00+08:00
lastmod: 2020-03-16T22:34:00+08:00
categories: ["技术"]
tags: ["git", "Windows"]
draft: false
---

## 说明

默认的 `git bash`配色实在太难看，所以在网上找了一下配色方案，结果都不满意。很多 `IDE`一直用的都是 `atom-one-dark`配色主题，所以Git也使用这个，特此记录。

### 步骤

配色使用的到颜色如下：（可根据自己喜好修改）

![color_reference](https://cdn.rumosky.com/usr/uploads/2020/03/1820195516.png)

先放一张VScode配色图：

![VScode配色](https://cdn.rumosky.com/usr/uploads/2020/03/1941264696.png)

好了，言归正传，下面我们开始配置 `git bash`

1. 打开 `git bash`，切换至用户目录，即 `cd ~`
2. 修改配置文件，即 `vi .minttyrc`
3. 配置文件内容修改如下：（若没有此文件，新建即可）


```bash
Font=Consolas
FontHeight=10

ForegroundColour=131,148,150
BackgroundColour=40,44,52
CursorColour=82,139,255

Black=40,44,52
BoldBlack=90,44,52
Red=224,108,117
BoldRed=255,108,117
Green=152,195,121
BoldGreen=152,245,121
Yellow=229,192,123
BoldYellow=229,192,173
Blue=97,175,239
BoldBlue=97,175,255
Magenta=198,120,221
BoldMagenta=248,120,221
Cyan=86,182,194
BoldCyan=86,232,194
White=171,178,191
BoldWhite=231,178,191
BoldAsFont=-1
FontSmoothing=full
FontWeight=700
Locale=C
Charset=UTF-8
Columns=80
Rows=28
```

重启git bash，效果如下：

![git bash主界面](https://cdn.rumosky.com/usr/uploads/2020/03/3435525962.png)

![git bash编辑文件界面](https://cdn.rumosky.com/usr/uploads/2020/03/3981738400.png)

#### 补充

> 使用 `vi`命令之后，是无法编辑文件的，按下 `insert`键，底部状态显示 `insert`则此时可以输入内容，保存时，请先按 `Esc`退出 `insert`状态，然后输入 `wq`保存并退出