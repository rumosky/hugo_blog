---
title: "Android版课程表APP"
slug: "80"
date: 2019-11-26T14:48:00+08:00
lastmod: 2019-11-26T14:48:00+08:00
categories: ["技术"]
tags: ["android"]
draft: false
---

## 引言

Android实践demo

源码：[ClassSchedule](https://github.com/rumosky/ClassSchedule)

### 内容

默认分支为`past`（旧版）

```txt
Gradle版本2.14.1
Gradle Plugin版本2.2.0
```

`master`分支（新版）

```txt
Gradle版本5.4.1
Gradle插件版本3.5.2
```

编译开发请自行选择相应版本，体验请下载发行版，app仅需要存储和相机权限

默认已配置Gradle源为阿里镜像 使用旧版可直接编译运行，无报错，新版由于API修改，会提示warning，可忽略

> 可以通过内网穿透加爬取校内数据库资源，实现从校务处导入课程，相关代码涉及学校，恕不提供