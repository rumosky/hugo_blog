---
title: "迁移Git项目"
slug: "117"
date: 2022-11-29T18:08:00+08:00
lastmod: 2022-11-29T18:08:00+08:00
categories: ["技术"]
tags: ["gitea", "git"]
draft: false
---

## 前言

之前使用的是GitHub和Gitee，国外的访问速度太慢，国内的又各种限制，所以自己搭建了一个Gitea，用来存放代码，之前的仓库不想丢失log记录等信息，所以需要迁移，特此记录。

### 步骤

本文示例Github仓库地址：`https://github.com/rumosky/demo.git`

第一步，在本地电脑上执行：

```bash
git clone --bare https://github.com/rumosky/demo.git
```

第二步，在新的远程仓库Gitea新建一个仓库，记录地址如下：

```bash
https://git.rumosky.com/rumosky/demo.git
```

第三步，进入刚刚克隆的demo仓库目录，执行：

```bash
git push --mirror https://git.rumosky.com/rumosky/demo.git
```

大功告成，访问新的远程仓库地址就可以看到原来的仓库代码、标签、历史记录都迁移过去了。

## 结语

Gitea默认是有迁移功能的，从Github或者其他Git平台都可以迁移，但是由于众所周知的网络问题，尝试了几次都提示从Github迁移失败，所以还是克隆到本地手动迁移比较方便

最后，Gitea 1.17.3迁移后发现一个BUG，侧边标签列表和顶部Git标签数据不同步，最新开发版1.19则没有这个问题，截至发文，已经反馈至Gitea官方，等待修复。