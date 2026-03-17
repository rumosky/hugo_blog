---
title: "Mac电脑删除后台运行程序"
slug: "719"
date: 2023-10-15T23:04:43+08:00
lastmod: 2023-10-15T23:04:43+08:00
categories: ["随笔"]
tags: ["Apple", "苹果", "Mac"]
draft: false
---

## 前言

卸载了软件，但是在登陆项里面还有后台运行程序存在，记录一下如何删除

### 步骤

后台运行程序在资源库的`LaunchAgents`和`LaunchDaemons`里面，具体路径如下：

```bash
~/Library/LaunchAgents
/Library/LaunchAgents
/Library/LaunchDaemons
```

这三个文件夹里面，自行搜索相应软件名，删除对应的`plist`文件即可，注意，一定要找对文件，不要误删，避免其他程序无法运行