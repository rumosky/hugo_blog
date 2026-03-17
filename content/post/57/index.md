---
title: "Python pip 更换镜像源"
slug: "57"
date: 2020-02-12T01:20:00+08:00
lastmod: 2020-02-12T01:20:00+08:00
categories: ["技术"]
tags: ["python", "Windows", "aliyun", "国内镜像"]
draft: false
---

## 说明

有时候网不好，pip安装非常慢，所以需要更换源，特记录如下

### 步骤

国内镜像地址：

```bash
# 清华大学
https://pypi.tuna.tsinghua.edu.cn/simple

# 豆瓣
https://pypi.douban.com/simple/

# 阿里云
https://mirrors.aliyun.com/pypi/simple/

# 中国科技大学
https://pypi.mirrors.ustc.edu.cn/simple/

# 华中理工大学
https://pypi.hustunique.com/

# 山东理工大学
https://pypi.sdutlinux.org/
```

本文从临时使用和永久使用分别介绍

#### 临时使用

使用pip时，加上参数`-i https://mirrors.aliyun.com/pypi/simple/`即可

例如

```bash
pip install pyspider -i https://mirrors.aliyun.com/pypi/simple/
```

这样就从阿里云镜像站安装pyspider

#### 永久使用

> Windows和Linux配置不同，分别介绍

##### Windows系统

直接在用户目录下新建pip文件夹，然后在此文件夹内新建文件`pip.ini`，内容如下：

```bash
[global]
index-url = https://mirrors.aliyun.com/pypi/simple/
[install]
trusted-host = mirrors.aliyun.com
```

注意：用户目录路径为`C:\Users\xxxx\`（xxx是计算机用户名）

**2022-08-04更新**：上述配置文件`pip.ini`放置路径修改为`C:\Users\xxx\AppData\Roaming\pip`


也可以直接使用下面的命令设置：

```bash
pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/
```

##### Linux系统

修改用户目录下的`.pip`文件夹内的`pip.conf`文件（若没有则新建`.pip/pip.conf`），文件内容和上面Windows系统一致。

> Linux下完整文件路径为`~/.pip/pip.conf`

## 结语

最后附上查看pip源命令

```bash
pip config list
```