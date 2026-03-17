---
title: "Docker指定latest标签"
slug: "665"
date: 2023-05-30T15:08:00+08:00
lastmod: 2023-05-30T15:08:00+08:00
categories: ["技术"]
tags: ["docker", "alist"]
draft: false
---

## 说明

之前发布了两个版本的docker文件，有1.0和2.0，默认使用latest标签应该指向最新的2.0版本，记录一下

### 正文

首先，之前已经发布过2.0版本了，构建的命令如下：

```bash
docker build -t rumosky/alist:2.0 .
```

推送

```bash
docker push rumosky/alist:2.0
```

现在手动将`latest`标签指向最新的2.0版本即可，执行：

```bash
docker tag rumosky/alist:2.0 rumosky/alist:latest
```

推送

```bash
docker push rumosky/alist:latest
```

## 最后

使用`latest`标签有一个潜在的问题是，当你构建并推送新的镜像时，上一次的最新镜像可能会被带有`latest`标签的新镜像所覆盖。因此，在实际生产中，建议使用更明确的标签来指定不同版本的镜像。