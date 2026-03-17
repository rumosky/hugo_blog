---
title: "Python新包管理工具uv的使用"
slug: "854"
date: 2025-11-20T11:40:00+08:00
lastmod: 2025-11-20T11:44:05+08:00
categories: ["随笔"]
tags: ["linux", "git", "python", "Windows"]
draft: false
---

之前一直用的poetry，但是这个不能管理Python版本，需要使用pyenv，这个原生支持的是linux和mac，Windows版本虽然也有一个pyenv-win，但总的还是有点麻烦，无意中看到有个rust写的uv，速度是真的快，而且可以一键管理Python版本，很省事。

## 安装

```bash
# On macOS and Linux.
curl -LsSf https://astral.sh/uv/install.sh | sh

# On Windows.
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"

# With pip.
pip install uv

# Or pipx.
pipx install uv
```

官方文档地址：https://docs.astral.sh/uv/

## 使用

```bash
# 查看可用的 Python 版本
uv python list

# 安装特定版本的 Python
uv python install 3.12

# 创建虚拟环境
uv venv

# 激活虚拟环境
source .venv/bin/activate # macOS/Linux
venv\Scripts\activate # Windows

# 为项目固定 Python 版本
uv python pin 3.11

# 安装包
uv pip install requests

# 升级包
uv pip upgrade requests

# 卸载包
uv pip uninstall requests

# 如果想全局系统级别使用，可以添加 --system参数:

uv pip install --system pandas

# 导出依赖
uv pip freeze > requirements.txt

# 初始化新项目
uv init my_project

# 安装项目依赖
uv sync

设置国内镜像源

[tool.uv]
index-url = "https://pypi.tuna.tsinghua.edu.cn/simple"
```