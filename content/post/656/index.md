---
title: "Windows11系统时间显示到秒"
slug: "656"
date: 2023-05-11T09:58:49+08:00
lastmod: 2023-05-11T09:58:49+08:00
categories: ["随笔"]
tags: ["Windows", "win11", "win10"]
draft: false
---

## 说明

Windows11系统时间默认不显示到秒，记录一下

### 正文

最新的Windows11已经支持开启显示到秒，设置方法如下：依次打开`设置 -> 个性化 -> 任务栏 -> 任务栏行为 `，在这个界面勾选`在系统托盘时钟中显示秒数`

如果没有这个选项，使用下面的方法开启


以管理员身份运行终端，执行：

```bash
powershell.exe Set-ItemProperty -Path HKCU:\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced -Name ShowSecondsInSystemClock -Value 1 -Force
```

此命令也适用于win10