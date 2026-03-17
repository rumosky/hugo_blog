---
title: "Ubuntu安装nodejs"
slug: "548"
date: 2023-04-27T16:50:00+08:00
lastmod: 2023-04-27T16:50:00+08:00
categories: ["技术"]
tags: ["linux", "nodejs", "nvm"]
draft: false
---

## 说明

如题，记录在Ubuntu安装nodejs过程

### 步骤

访问nodejs官网：[nodejs下载页][1]

本文以18.16为例

1，下载

```bash
wget https://nodejs.org/dist/v18.16.0/node-v18.16.0-linux-x64.tar.xz
```

2，解压缩

```bash
tar -xvf node-v18.16.0-linux-x64.tar.xz
```

3，添加环境变量

```bash
vi /etc/profile
```

4，在文件最后添加如下内容：

```bash
export PATH=$PATH:/usr/node-v18.16.0-linux-x64/bin
```

PATH后面为nodejs路径

5，应用

```bash
source /etc/profile
```

此时，输入node，即可发现成功安装


  [1]: https://nodejs.org/en/download