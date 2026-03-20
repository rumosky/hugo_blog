---
title: "PicGo壓縮圖片外掛程式"
slug: "882"
date: 2026-03-20T11:46:00+08:00
lastmod: 2026-03-20T11:46:00+08:00
categories: ["技術"]
tags: ["linux", "Windows"]
draft: false
---

## 介紹

最近使用picgo作為圖片的管理工具，在插件市場裡面试了compress1.4和compress-next1.5.2版本，還有tinypng的插件，但是都無法正常使用，不知道為啥，所以自己寫了一個插件。

## 使用

![插件圖標](https://cdn.rumosky.com/usr/uploads/2026/03/202603208736.png)

[picgo-plugin-compress-pro](https://github.com/rumosky/picgo-plugin-compress-pro)

1. 支持**本地壓縮**和**在線壓縮**。

本地壓縮使用`pngquant`和`jpegoptim`兩個庫，在線壓縮使用tinypng。

2. 支持以下圖片格式的壓縮：

- PNG（使用 pngquant 壓縮）
- JPG/JPEG（使用 jpegoptim 壓縮）
- TinyPNG（透過 TinyPNG API 支援 PNG 和 JPG 格式的壓縮）

## 使用

![設置](https://cdn.rumosky.com/usr/uploads/2026/03/202603208713.png)

1. 選擇本地模式，請設置`pngquant`和`jpegoptim`的路徑。結尾請添加`/`。
2. 選擇線上模式，請設置tinypng的API Key。
