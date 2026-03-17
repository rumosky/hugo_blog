---
title: "uv使用国内镜像"
slug: "856"
date: 2025-11-20T15:03:00+08:00
lastmod: 2025-12-24T18:14:07+08:00
categories: ["随笔"]
tags: []
draft: false
---

按照文档配置，内容如下：

```toml
python-install-mirror = "https://registry.npmmirror.com/-/binary/python-build-standalone/"

[[index]]
url = "https://mirrors.aliyun.com/pypi/simple"
default = true

[pip]
index-url = "https://mirrors.aliyun.com/pypi/simple"
```