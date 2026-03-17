---
title: "Git使用教程"
slug: "38"
date: 2019-07-24T23:34:00+08:00
lastmod: 2019-07-24T23:34:00+08:00
categories: ["随笔"]
tags: ["linux", "git", "Windows"]
draft: false
---

## 介绍

多人协作开发项目的时候，都是使用GitHub或者GitLab等，新手踩了很多坑，所以做此记录，方便查阅。 Git是目前世界上最先进的分布式版本控制系统（没有之一），更多内容就请自行百度。

### 安装

地址：[Git](https://git-scm.com/downloads "Git")

下载之后默认安装即可

作者写有 [Git学习手册](https://rumosky.net/books/learngit "Git学习手册")，十分全面，欢迎阅读！

### 常用命令

```bash
git clone
# 克隆远程仓库

git init
# 初始化一个仓库

git add
# 将文件修改添加到缓冲区

git mv
# 移动或重命名一个文件、文件夹或快捷方式

git reset
# 回退项目版本

git rm
# 将文件修改从缓冲区中移除

git status
# 显示项目当前状态

git log
# 显示项目日志

git branch
# 显示项目分支

git checkout
# 切换分支或重置文件

git commit
# 提交项目修改到仓库

git diff
# 对比版本之间、版本和当前工作状态之间的差异

git merge
# 合并文件

git rebase
# 将新的提交放在另一个分支的上面

git tag
# 创建、显示、校验标签对象

git fetch
# 拉取其他仓库的对象和索引

git pull
# 拉取其他仓库内容并和本地分支合并

git push
# 更新远程仓库
```

### 说明

在你的电脑上新建一个文件夹或者使用`git clone`命令远程下载仓库到本地，这时，本地的文件夹叫做本地仓库；与之对应，放在GitHub或Gitee等代码托管平台的仓库称为远程仓库。本文推荐使用`Git`命令行来操作，不推荐使用图形化界面。所有操作流程也仅提供命令行模式，图形化操作方式请自行百度。

#### 使用流程

一般使用情况有两种，一是远程仓库已经存在，在本地电脑上新建文件夹，然后克隆远程仓库到本地进行开发，然后再上传到远程仓库；二是远程仓库不存在，已经有本地项目文件，将本地项目文件初始化为一个git仓库，同时在新建一个远程仓库，将本地仓库上传到远程仓库。

下面是具体操作流程：

##### 情况1

1.在本地电脑任意位置新建一个文件夹，打开`Git`命令行或`cmd`，执行：

```bash
git clone url
# url为你的远程仓库http地址
```

2.克隆成功之后，在你新建的文件夹下面会产生于一个与远程仓库同名的文件夹，这就是本地仓库。

3.在本地仓库修改之后，执行：

```bash
git add . 
# . 的意思是提交全部修改过的文件到缓冲区

git add /xxx/xxx/xxx.html
# 后面的 /xxx/xxx/xxx.html为具体的某一个文件路径

# 以上两个提交方法请自行选择使用
```

4.提交之后执行：

```bash
git commit -m "修改了css全局样式"
# ""中的内容为你的此次提交的说明，中英文随意
```

5.若此项目仅由一个人开发，再无其他开发者，则执行：

```bash
git push origin master
```

6.若此项目由多个开发者，则依次执行：

```bash
git pull
# 此操作是拉取最新更新，防止别人在你提交之前有了新的更改

git push origin master
# 拉取完成后，直接push到远程仓库
```

##### 情况2

与情况1相比，情况2仅需要在本地项目文件夹内执行初始化命令`git init`，此时，本地文件夹就成为了本地仓库，然后执行了`git add`和`git commit`之后，将本地仓库与远程仓库想关联，执行`git remote add origin 远程仓库url`，然后即可push或者pull了

### 补充

`git commit`、`git push`、`git pull`、 `git fetch`、`git merge`的含义与区别

- git commit：是将本地修改过的文件提交到本地库中；
- git push：是将本地库中的最新信息发送给远程库；
- git pull：是从远程获取最新版本到本地，并自动merge；
- git fetch：是从远程获取最新版本到本地，不会自动merge；
- git merge：是用于从指定的commit合并到当前分支，用来合并两个分支；

<!-- -->

**git pull** 相当于 **git fetch + git merge**

> 有时`git pull`之后，会弹出一个vim编辑器页面，请自动忽略，退出即可，继续push就行
> 这时弹出的vim是让你记录合并分支的信息，保存或退出都可以
> 退出vim方法：输入`:q`按回车即可

## 相关推荐

1. [Git 英文教程（基本原理）]<https://jwiegley.github.io/git-from-the-bottom-up/ "Git 英文教程（基本原理）")
2. [Git 中文教程（命令用法）](https://github.com/lonelydawn/git-recipes/ "Git 中文教程（命令用法）")
3. [Git GUI 推荐：Source Tree](https://www.sourcetreeapp.com/ "Git GUI 推荐：Source Tree")
4. [GitHub官方GUI](https://desktop.github.com/ "GitHub官方GUI")
5. [GitHub](https://github.com/ "GitHub")
6. [BitBucket](https://bitbucket.org/ "BitBucket")
7. [Gitee](https://gitee.com/ "Gitee")