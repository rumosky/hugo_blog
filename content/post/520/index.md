---
title: "MacOS更新Git"
slug: "520"
date: 2023-03-05T21:33:06+08:00
lastmod: 2023-03-05T21:33:06+08:00
categories: ["技术"]
tags: ["git", "Apple", "苹果", "Mac", "homebrew"]
draft: false
---

## 说明

最近使用Mac电脑，发现Git不知道怎么升级，查了一下，特此记录

### 步骤

首先，在terminal执行

```bash
which git
/usr/bin/git
```

会显示

```bash
/usr/bin/git
```

再执行

```bash
git --version
```

会显示

```bash
git version 2.37.1 (Apple Git-137.1)
```

这个时候用的Xcode上的Git，再执行

```bash
brew install git
```

```bash
HOMEBREW_BREW_GIT_REMOTE set: using https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git as the Homebrew/brew Git remote.
remote: Enumerating objects: 326, done.
remote: Counting objects: 100% (326/326), done.
remote: Compressing objects: 100% (48/48), done.
remote: Total 631 (delta 305), reused 278 (delta 278), pack-reused 305
Receiving objects: 100% (631/631), 307.39 KiB | 17.08 MiB/s, done.
Resolving deltas: 100% (331/331), completed with 94 local objects.
From https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew
   10dd6a30c..8051e8818  master     -> origin/master
HOMEBREW_CORE_GIT_REMOTE set: using https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git as the Homebrew/homebrew-core Git remote.
Running `brew update --auto-update`...
remote: Enumerating objects: 1184, done.
remote: Counting objects: 100% (1184/1184), done.
remote: Compressing objects: 100% (98/98), done.
remote: Total 1673 (delta 1162), reused 1086 (delta 1086), pack-reused 489
Receiving objects: 100% (1673/1673), 1.10 MiB | 22.95 MiB/s, done.
Resolving deltas: 100% (1192/1192), completed with 223 local objects.
From https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core
   f8376785ae6..78c20992775  master     -> origin/master
==> Fetching dependencies for git: gettext and pcre2
==> Fetching gettext
==> Downloading https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles/gettext-0.
######################################################################## 100.0%
==> Fetching pcre2
==> Downloading https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles/pcre2-10.4
######################################################################## 100.0%
==> Fetching git
==> Downloading https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles/git-2.39.2
######################################################################## 100.0%
==> Installing dependencies for git: gettext and pcre2
==> Installing git dependency: gettext
==> Pouring gettext-0.21.1.arm64_ventura.bottle.tar.gz
🍺  /opt/homebrew/Cellar/gettext/0.21.1: 1,983 files, 20.9MB
==> Installing git dependency: pcre2
==> Pouring pcre2-10.42.arm64_ventura.bottle.tar.gz
🍺  /opt/homebrew/Cellar/pcre2/10.42: 230 files, 6.2MB
==> Installing git
==> Pouring git-2.39.2.arm64_ventura.bottle.1.tar.gz
==> Caveats
The Tcl/Tk GUIs (e.g. gitk, git-gui) are now in the `git-gui` formula.
Subversion interoperability (git-svn) is now in the `git-svn` formula.

zsh completions and functions have been installed to:
  /opt/homebrew/share/zsh/site-functions
==> Summary
🍺  /opt/homebrew/Cellar/git/2.39.2: 1,627 files, 48MB
==> Running `brew cleanup git`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).
==> Caveats
==> git
The Tcl/Tk GUIs (e.g. gitk, git-gui) are now in the `git-gui` formula.
Subversion interoperability (git-svn) is now in the `git-svn` formula.

zsh completions and functions have been installed to:
  /opt/homebrew/share/zsh/site-functions
```

重新指定新Git地址

```bash
brew link git --overwrite
```

显示如下：

```bash
==> Downloading https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles/api/formul
##O#-#                                                                        
Warning: Already linked: /opt/homebrew/Cellar/git/2.39.2
To relink, run:
  brew unlink git && brew link git
```

关闭terminal，重新查看git版本，执行

```bash
git --version
```

会显示如下：

```bash
git version 2.39.2
```

此时已经是当前最新版本了，再核查路径：

```bash
which git
```

显示如下：

```bash
/opt/homebrew/bin/git
```

显示路径与原来的Git安装路径一致。至此，Git版本升级完毕。