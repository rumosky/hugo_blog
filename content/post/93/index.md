---
title: "腾讯云COS跨域配置"
slug: "93"
date: 2021-11-26T16:04:00+08:00
lastmod: 2021-11-26T16:04:00+08:00
categories: ["技术"]
tags: ["CDN", "腾讯云", "COS"]
draft: false
---

## 说明

之前用腾讯云COS的时候，出现了跨域问题，一直报403问题。

### 正文

其余的配置就不说了，参考：[bspost cid="92"]

跨域的配置在这里：CDN的配置如下：

![CDN](https://cdn.rumosky.com/usr/uploads/2023/05/3472032116.png)

COS的配置：安全管理 -> 跨域访问CORS设置，配置如下：

![COS](https://cdn.rumosky.com/usr/uploads/2023/05/3433366875.png)

上述两个配置修改好之后，就可以正常访问了