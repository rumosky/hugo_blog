---
title: "typecho博客handsome主题Gitee独立页面（基于GitHub独立页面修改）"
slug: "11"
date: 2019-01-01T10:26:00+08:00
lastmod: 2019-01-01T10:26:00+08:00
categories: ["随笔"]
tags: ["typecho"]
draft: false
---

## 说明

本站之前用的采用typecho程序，handsome主题，主题内置了github的独立页面，可以显示你的公开仓库。但是，重点来了，由于某些因素，国内github访问速度十分慢，所以，有时经常出现一直卡在`加载中……`的情况

### 源码

国内地址：https://gitee.com/rumosky/gitee_handsome

#### 使用方法

> 1，将源代码下载到本地，文件名修改为gitee.php，然后上传到typecho博客主题文件夹的根目录下； 
> 2，在typecho博客后台新建独立页面，在右侧的模板里面选择gitee项目列表，在底部添加字段，字段名称是gitee，字段值是你的gitee用户名，然后发布独立页面即可。

此时在前台访问刚刚发布的独立页面即可访问gitee项目列表