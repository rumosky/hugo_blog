---
title: "git pull 报错：Pulling in not possible because you have unmerged files"
slug: "42"
date: 2019-10-15T00:43:00+08:00
lastmod: 2019-10-15T00:43:00+08:00
categories: ["技术"]
tags: ["linux", "git", "Windows"]
draft: false
---

## 说明

如题，在使用git的时候出现了`Pulling in not possible because you have unmerged files`的问题，记录一下解决办法

### 解决办法

1.`git pull`会产生`merge`操作导致冲突，需要将冲突的文件resolve掉之后才能成功`pull`

所以执行下列命令即可：

```bash
git add -u
git commit -m "注释"
git pull
```

2.放弃本地更改也可以解决冲突

放弃本地更改`git reset --hard FETCH_HEAD`，其中`FETCH_HEAD`表示上一次成功`pull`之后的`commit`点，然后`git pull`即可