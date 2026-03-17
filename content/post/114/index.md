---
title: "nvm一个多版本nodejs管理工具"
slug: "114"
date: 2022-11-21T13:41:00+08:00
lastmod: 2022-11-21T13:41:00+08:00
categories: ["技术"]
tags: ["nodejs", "nvm"]
draft: false
---

## 说明

平时用的nodejs是16版本，项目上用的是14，偶尔还会需要使用最新的18，所以本文记录一下nvm工具使用。

### 步骤

nvm下载地址：[nvm官网](https://nvm.uihtm.com/)

**注**：建议国内使用

nvm源码：[bsgit user="coreybutler"]nvm-windows[/bsgit]

#### 配置

具体步骤在这里就不说了，只提一下不一样的地方，官网上面的步骤很详细了

在使用`nvm use`的时候，需要使用管理员权限打开命令行，否则会报`statius 1`的错误

安装位置请勿选择有空格的目录

下载速度过慢或者`nvm list available`时表格显示为空，请使用国内镜像地址，具体方法，在nvm安装目录下`settings.txt`中添加：

```bash
node_mirror: https://npmmirror.com/mirrors/node/
npm_mirror: https://npmmirror.com/mirrors/npm/
```

> 注意，官网上的国内地址为旧域名，建议使用上述内容

## 结语

这个工具不能分别指定项目使用不同版本nodejs，只能是安装多个版本，随时切换。暂时没发现什么特别大的BUG