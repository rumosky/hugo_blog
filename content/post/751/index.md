---
title: "WSL子系统Ubuntu修改文件提示没权限"
slug: "751"
date: 2024-02-27T22:36:00+08:00
lastmod: 2024-02-27T22:36:52+08:00
categories: ["技术"]
tags: ["Windows", "WSL", "ubuntu"]
draft: false
---

## 说明

如题，在vscode中修改Ubuntu文件发现没有权限，记录一下。

### 步骤

很简单，使用下面命令赋予对应目录用户权限，请自行替换用户名和目录名

```bash
sudo chown -R username /your/dirname
```