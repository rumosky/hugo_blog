---
title: "Git学习手册"
slug: "55"
date: 2019-12-12T01:15:00+08:00
lastmod: 2019-12-12T01:15:00+08:00
categories: ["随笔"]
tags: ["git", "Windows"]
draft: false
---

## 说明

Git是目前最流行的版本控制系统，很多项目都在使用，记录学习历程。

### 内容

手册目录以及每章节所用的命令图示如下：

![Git速查手册目录示意图](https://cdn.rumosky.com/usr/uploads/2019/12/1610118053.png)

地址：[Git学习手册](https://rumosky.net/books/learngit/ "Git学习手册")

根据廖雪峰Git教程改编，结合学习过程中的很多疑问，综合了许多留言者的常见问题。

> 上图是最初版，最新内容，请访问上述链接

#### 优点

- 廖雪峰Git教程都是先讲实例，最后得出结论，告诉相关命令。本手册先写结论，后讲述原理
- 解决了相关廖雪峰未讲述清楚的原理
- 使用最新版Git，更新了switch restore命令
- 详细说明了相关配置过程与可能遇到的问题


其中，最重要的扩展知识内容如下：

```bash
# git 的 merge 与 no-ff merge 的不同之处
# git报错fatal: refusing to merge unrelated histories
# git merge origin master与merge origin/master的区别
# Git撤回已经推送至远程仓库的提交
# 浅析warning: LF will be replaced by CRLF
# git pull 报错：Pulling in not possible because you have unmerged files
```

提供**中英双语**版本！