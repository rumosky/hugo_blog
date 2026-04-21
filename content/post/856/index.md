---
title: "uv使用国内镜像"
slug: "856"
date: 2025-11-20T15:03:00+08:00
lastmod: 2025-12-24T18:14:07+08:00
categories: ["随笔"]
tags: []
draft: false
---

[官方配置文档地址](https://docs.astral.sh/uv/concepts/configuration-files/)

> user-level configuration (e.g., ~/.config/uv/uv.toml) and system-level configuration (e.g., /etc/uv/uv.toml).

按照文档配置，内容如下：



```toml

python-install-mirror = "https://registry.npmmirror.com/-/binary/python-build-standalone/"



[[index]]

url = "https://mirrors.aliyun.com/pypi/simple"

default = true



[pip]

index-url = "https://mirrors.aliyun.com/pypi/simple"

```

Windows的配置文件默认位置为`C:\Users\你的用户名\AppData\Roaming\uv\uv.toml`


## 补充

可以使用命令，临时指定配置文件

```bash
# 这里的路径换成你实际的配置文件路径
uv lock --config-file "C:\Users\YourName\.config\uv\uv.toml"
```

使用环境变量，powershell临时：

```bash
$env:UV_CONFIG_FILE = "C:\Users\YourName\.config\uv\uv.toml"
# 然后直接运行，它会自动读取该配置
uv lock
```

```bash
$env:UV_INDEX_URL = "https://mirrors.aliyun.com/pypi/simple"
uv lock
```

也可以直接把配置文件设置为系统环境变量，配置好之后一劳永逸