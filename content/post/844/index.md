---
title: "python poetry配置国内镜像"
slug: "844"
date: 2024-12-19T22:42:00+08:00
lastmod: 2025-11-04T15:21:33+08:00
categories: ["技术"]
tags: ["python", "poetry"]
draft: false
---

## 说明

poetry默认的源总是超时，修改一下国内源。

### 步骤

可以在项目里面配置，也可以全局配置。

#### 项目配置

在项目的`pyproject.toml`里添加以下内容：

```bash
[[tool.poetry.source]]
name = "aliyun"
url = "https://mirrors.aliyun.com/pypi/simple"
priority = "primary"

[[tool.poetry.source]]
name = "tsinghua"
url = "https://pypi.tuna.tsinghua.edu.cn/simple/"
priority = "supplemental"
```


`priority`可选的值是：`primary`, `supplemental`, `explicit`

#### 全局配置

在系统环境变量里添加一个`POETRY_INDEX_URL`的变量，值为对应的镜像地址