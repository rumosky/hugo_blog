---
title: "如何避免git pull时产生Merge branch master of等类似操作"
slug: "43"
date: 2019-10-15T00:45:00+08:00
lastmod: 2019-10-15T00:45:00+08:00
categories: ["技术"]
tags: ["linux", "git", "Windows"]
draft: false
---

## 介绍

如题，在使用 Git 的进行代码版本控制的时候，往往会发现在 log 中出现 “Merge branch ‘master’ of …” 这句话，如下图所示。日志中记录的一般为开发过程中对代码的改动信息，如果出现过多例如上述描述的信息会造成日志的污染。

如图，此图为VS code中git history插件图

![git history](https://cdn.rumosky.com/usr/uploads/2019/07/412822696.png)

可以看到主分支`不干净`，节点很多、很乱

### 产生的原因

当多人合作开发一个项目时，本地仓库落后于远程仓库是一个非常正常的事情，可参考下图。

```txt
A-B-C(master)
\
D(origin/master)
```

具体情境如下：

我当前拉取的远端版本为 B，此时修改了代码，并在本地仓库 commit 一次，但并未 push 到远端仓库。 另一位开发者在 B 的基础上，同样 commit 了一次并 push 到远端仓库。那么这个时候，我再 push 自己的代码就会发生错误

这个时候我们会选择，先 pull，再 push。Ok，push 成功，但是此时我们查看 log 就会发现除了我们自己提交的那条日志之外，会多出一条 “Merge branch ‘master’ of …”。

那么，为什么会出现这种现象呢？其实是与 Git 的工作原理有关，对 Git 比较了解的人应该会知道，无论是 pull、push 亦或是 merge 操作，其实背后都是有很多的不同的模式的。

在进行 pull 操作的同时，其实就是 fetch+merge 的一个过程。我们从 remote 分支中拉取新的更新，然后再合并到本地分支中去。

如果 remote 分支超前于本地分支，并且本地分支没有任何 commit 的，直接从 remote 进行 pull 操作，默认会采用 fast-forward 模式，这种模式下，并不会产生合并节点，也就是说不会产生多余的那条 log 信息 如果想之前那样，本地先 commit 后再去 pull，那么此时，remote 分支和本地会分支会出现分叉，这个时候使用 pull 操作拉取更新时，就会进行分支合并，产生合并节点和 log 信息。这两种状态分别如下所示：

```txt
# fast-forword 
A-B-D(origin/master)
     \
      C(master)
```

```txt
# merge
A-B-C-E(master)
   \ /
    D(origin/master)
```

### 如何解决

通过百度，包括自己的实践，有三种方法

#### 方法1

在执行`git pull`的时候加上`--rebase`参数，成功后在进行真正的`merge`操作。(如果有冲突需要手动解决)

#### 方法2

这个方法操作以后可以一劳永逸。在你的 git bash 里执行`git config --global pull.rebase true`，这个配置就是告诉 git 在每次 pull 前先进行 rebase 操作。这种方法和方法1原理一样，只不过方法1是每次pull前都要手动操作。

#### 方法3

不建议直接`pull`操作，建议在`commit`之后，先`fetch`一下，然后`merge`合并，最后执行 `push`命令

## 总结

> rebase 使用不好会在 git bash 顶部出现`(master|REBASE 1/1)`，此时执行`git rebase --abort`即可恢复
> rebase 操作的的好处是你们团队的 commit message 时间线会成一条笔直的直线