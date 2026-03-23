---
title: "PicGo压缩图片插件"
slug: "882"
date: 2026-03-20T11:46:00+08:00
lastmod: 2026-03-20T11:46:00+08:00
categories: ["技术"]
tags: ["linux", "Windows"]
draft: false
---

## 介绍

最近使用picgo作为图片的管理工具，在插件市场里面试了compress1.4和compress-next1.5.2版本，还有tinypng的插件，但是都无法正常使用，不知道为啥，所以自己写了一个插件。

## 使用

![插件图标](https://cdn.rumosky.com/usr/uploads/2026/03/202603208736.png)

插件源码：[picgo-plugin-compress-pro](https://github.com/rumosky/picgo-plugin-compress-pro)

1. 支持**本地压缩**和**在线压缩**。

本地压缩使用`pngquant`和`jpegoptim`两个库，在线压缩使用tinypng。

2. 支持以下图片格式的压缩：

- PNG（使用 pngquant 压缩）
- JPG/JPEG（使用 jpegoptim 压缩）
- TinyPNG（通过 TinyPNG API 支持 PNG 和 JPG 格式的压缩）

## 使用

![设置](https://cdn.rumosky.com/usr/uploads/2026/03/202603208713.png)

1. 选择本地模式，请设置`pngquant`和`jpegoptim`的路径。结尾请添加`/`。
2. 选择在线模式，请设置tinypng的API Key。
