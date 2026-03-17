---
title: "Mac电脑启动台图标无法删除"
slug: "718"
date: 2023-10-15T21:56:01+08:00
lastmod: 2023-10-15T21:56:01+08:00
categories: ["随笔"]
tags: ["Apple", "苹果", "Mac"]
draft: false
---

## 说明

安装了一个PS，结果附带了一堆乱七八糟的东西，卸载软件之后还有东西残留，记录一下

### 步骤

正常启动台图标，长按就可以删除，这种都是app store安装的，就可以。但自己用安装包安装的，删除不了，需要卸载。找了很多方法，很简单，执行下面的语句，就可以删除

```bash
sqlite3 $(find /private/var/folders \( -name com.apple.dock.launchpad -a -user $USER \) 2> /dev/null)/db/db "DELETE FROM apps WHERE title='xxx';" && killall Dock
```

这个`title="xxx"`里的`xxx`就是你需要删除的图标名称，执行即可