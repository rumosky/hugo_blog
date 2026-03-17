---
title: "MacOS配置homebrew（新版）"
slug: "714"
date: 2023-10-14T10:59:00+08:00
lastmod: 2023-10-14T10:59:00+08:00
categories: ["技术"]
tags: ["linux", "Apple", "苹果", "homebrew"]
draft: false
---

## 前言

之前写过一篇配置homebrew的文章，这两天重装了系统，又需要重新配置，发现官网上很多东西修改了，记录一下。

### 配置

首先，放上清华源的说明：

*注：自brew 3.3.0 (2021 年 10 月 25日) 起，Linuxbrew 核心仓库`linuxbrew-core`已被弃用。Linuxbrew用户应迁移至`homebrew-core`，并请依本镜像使用帮助重新设置镜像。具体请参阅本帮助`Linuxbrew镜像迁移说明`章节。*

*注：自brew 4.0.0 (2023 年 2 月 16日) 起，`HOMEBREW_INSTALL_FROM_API`会成为默认行为，无需设置。大部分用户无需再克隆`homebrew-core`仓库，故无需设置`HOMEBREW_CORE_GIT_REMOTE`环境变量；但若需要运行`brew`的开发命令或者 `brew`安装在非官方支持的默认 prefix 位置，则仍需设置`HOMEBREW_CORE_GIT_REMOTE`环境变量。如果不想通过 API 安装，可以设置`HOMEBREW_NO_INSTALL_FROM_API=1`。*

*注：自brew 4.0.22 (2023 年 6 月 12 日) 起，`homebrew-cask-drivers`已被弃用，所有 cask 合并至`homebrew-cask`仓库。本帮助内已移除克隆`homebrew-cask-drivers`仓库的命令。已克隆用户可选择运行`brew untap homebrew/cask-drivers`命令移除此仓库。*

清华源homebrew地址：https://mirrors.tuna.tsinghua.edu.cn/help/homebrew/

在当前用户目录下，先添加环境变量：(默认是zsh不是bash，注意区分)

```bash
export HOMEBREW_INSTALL_FROM_API=1
export HOMEBREW_API_DOMAIN="https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles/api"
export HOMEBREW_BOTTLE_DOMAIN="https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles"
export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git"
export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git"
export HOMEBREW_PIP_INDEX_URL="https://pypi.tuna.tsinghua.edu.cn/simple"
```

最后这一条Python的可添加也可不添加，以上内容填写好之后，保存`.zprofile`文件，然后执行`source .zprofile`即可

验证是否配置成功，请执行`brew update`

#### 阿里云镜像源

如果所在地区访问清华源比较慢，可以使用阿里云，配置地址如下：

```bash
export HOMEBREW_INSTALL_FROM_API=1
export HOMEBREW_API_DOMAIN="https://mirrors.aliyun.com/homebrew/homebrew-bottles/api"
export HOMEBREW_BOTTLE_DOMAIN="https://mirrors.aliyun.com/homebrew/homebrew-bottles"
export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.aliyun.com/homebrew/brew.git"
export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.aliyun.com/homebrew/homebrew-core.git"
export HOMEBREW_PIP_INDEX_URL="https://mirrors.aliyun.com/pypi/simple"
```

### 最后

附上之前写的文章，内含国内一键安装脚本，适用于无法访问GitHub的情况：
[bspost cid="517"]