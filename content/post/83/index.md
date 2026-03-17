---
title: "简易通讯录C#窗体程序"
slug: "83"
date: 2020-03-27T15:13:00+08:00
lastmod: 2020-03-27T15:13:00+08:00
categories: ["技术"]
tags: ["addressbook", "C#"]
draft: false
---

## 引言

可视化编程随手demo

源码：[addressbook](https://github.com/rumosky/addressbook)

### 内容

软件为简易通讯录，Windows 窗体程序，VS2019 版，NET Framework 4.7.2，MySQL 5.7.24

开发环境：

```txt
visual studio 2019；
NET Framework 4.7.2；
MySQL 5.7.24；
```

前往引言部分仓库中的`Release`下载`Setup.exe`，安装后即可预览（相关数据库已关闭，完整功能请自行使用源码配置数据库后使用）

#### 编译

数据库信息： 表名：`addressbook` 字段：`uid`（自增主键，不为空）`name`，`sex`，`phone`，`email`

修改 `ManageDatabase.cs` 文件中下面内容为自己的数据库地址：

```cs
String url = "server=127.0.0.1;port=3306;user=root;password=test@1234; database=classofc;Charset=utf8;";
```

#### 其他

帮助文档使用`easyCHM`生成，可执行文件使用`innosetup`打包