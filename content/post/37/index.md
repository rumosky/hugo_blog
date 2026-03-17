---
title: "同步更新Gitee和GitHub仓库代码"
slug: "37"
date: 2019-07-23T23:32:00+08:00
lastmod: 2019-07-23T23:32:00+08:00
categories: ["技术"]
tags: ["linux", "gitea", "git", "Windows"]
draft: false
---

## 说明

学校的网络不好，GitHub有时就上不去了，所以需要同步更新Gitee和GitHub两个仓库的代码，做个备份。

### 实现方式

百度了一下，只需要Git连接多个远程仓库地址就行，本文主要内容来源地址找不到了，代码是根据Git的记录而来，若有问题，请留言评论

#### 方法1-添加多个远程仓库

比如要链接两个 Github 仓库，分别是 github1 和 github2，那么：

```bash
# 添加 github1
git remote add github1 https://github.com/username/github1.git

# 添加 github2
git remote add github2 https://github.com/username/github2.git

# 提交到 github1
git push github1 master

# 提交到 github2
git push github2 master

# 从 github1 更新
git pull github1 master

# 从 github2 更新
git pull github2 master
```

#### 方法2-添加同名多个远程仓库

```bash
# 添加一个远程仓库
git remote add origin https://github.com/username/github1.git

# 然后分别设定push URL
git remote set-url --add --push origin https://github.com/username/github1.git
git remote set-url --add --push origin https://github.com/username/github2.git

# 检查远程仓库配置
git remote -v

# 若配置正确，则结果应当包含一个fetch路径和两个push路径

# 向所有远程仓库推送
git push origin master
```

#### 方法3-直接修改.git/config 文件

用文本编辑器打开本地仓库的 .git/config 文件，然后修改其中的远程仓库配置

```bash
# 假设当前的远程仓库名为 origin
[remote "origin"]
    url = https://github.com/username/github1.git
    fetch = +refs/heads/*:refs/remotes/github/*
    pushurl = https://github.com/username/github1.git
    pushurl = https://github.com/username/github2.git
```

然后直接使用

```bash
git push origin master
```

即可提交至所有版本库