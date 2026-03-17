---
title: "WSL子系统Ubuntu提示code .命令不存在"
slug: "749"
date: 2024-02-27T22:15:00+08:00
lastmod: 2024-02-27T22:36:41+08:00
categories: ["技术"]
tags: ["Windows", "vscode", "WSL", "ubuntu"]
draft: false
---

## 说明

今天在Ubuntu下使用`code .`命令唤醒vscode结果提示命令不存在，not found，记录一下解决方式。

### 步骤

默认VScode安装路径为：`C:\Program Files\Microsoft VS Code`

网上很多解决方案说，在Windows上，打开vscode，然后按`Ctrl+Shift+P`，然后输入`code`，选择`Sell Command: Install 'code' command in PATH`，但实际上，根本找不到这个选项。

> 如果在Windows终端下，找不到`code .`这个命令，则可以使用上面说的方式安装这个命令，但连接了WSL就不可以这样。

首先，确保vscode上安装了WSL这个扩展，安装上这个扩展之后，打开Ubuntu子系统，配置环境变量`vi /etc/profile`如下：

```bash
export VSCODE=/mnt/c/'Program Files'/'Microsoft VS Code'/bin
export PATH=$VSCODE:$PATH
```

保存之后，执行`source /etc/profile`，再执行`code .`，会输出一个百分比进度，100%之后显示如下内容：

```bash
Installing VS Code Server for x64 (903b1e9d8990623e3d7da1df3d33db3e42d80eda)
Downloading: 100%
Unpacking: 100%
Unpacked 1530 files and folders to /root/.vscode-server/bin/903b1e9d8990623e3d7da1df3d33db3e42d80eda.
```

此时，会自动打开vscode，并且会自动连接Ubuntu

## 结语

注意环境变量里面的路径有空格，所以需要加单引号，否则会报错