---
title: "添加cmd和powershell到右键菜单的一键脚本"
slug: "53"
date: 2019-11-16T01:10:00+08:00
lastmod: 2025-06-06T10:13:11+08:00
categories: ["技术"]
tags: ["Windows", "cmd", "powershell"]
draft: false
---

## 说明

为了方便起见，将cmd或powershell添加到右键菜单，管理员和非管理员权限，特记录于此。

### 脚本

此处记录脚本内容，注意分号`;`之后的内容是注释

#### cmd（非管理员）

```bash
Windows Registry Editor Version 5.00

; 添加到桌面右键菜单

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Directory\background\shell\cmd_here]

@="在此处打开命令行"
"Icon"="cmd.exe"

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Directory\background\shell\cmd_here\command]

@="\"C:\\Windows\\System32\\cmd.exe\""

; 添加到文件夹右键菜单

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Directory\shell\cmd_here]

@="在此处打开命令行"
"Icon"="cmd.exe"

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Directory\shell\cmd_here\command]

@="\"C:\\Windows\\System32\\cmd.exe\""
```

#### Windows PowerShell（非管理员）

```bash
Windows Registry Editor Version 5.00

; 添加到桌面右键菜单

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Directory\background\shell\powershell_here]

@="在此处打开PowerShell"
"Icon"="powershell.exe"

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Directory\background\shell\powershell_here\command]

@="\"C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe\""

; 添加到文件夹右键菜单

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Directory\shell\powershell_here]

@="在此处打开PowerShell"
"Icon"="powershell.exe"

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Directory\shell\powershell_here\command]

@="\"C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe\""
```

#### cmd（管理员）

```bash
Windows Registry Editor Version 5.00

; 添加到桌面右键菜单

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Directory\background\shell\cmd_admin_here]

@="在此处打开命令行（管理员）"
"Icon"="cmd.exe"

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Directory\background\shell\cmd_admin_here\command]

@="PowerShell -windowstyle hidden -Command \"Start-Process cmd.exe -ArgumentList '/s,/k, pushd,%V' -Verb RunAs\""

; 添加到文件夹右键菜单

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Directory\shell\cmd_admin_here]

@="在此处打开命令行（管理员）"
"Icon"="cmd.exe"

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Directory\shell\cmd_admin_here\command]

@="PowerShell -windowstyle hidden -Command \"Start-Process cmd.exe -ArgumentList '/s,/k, pushd,%V' -Verb RunAs\""
```

#### Windows PowerShell（管理员）

```bash
Windows Registry Editor Version 5.00

; 添加到桌面右键菜单

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Directory\background\shell\powershell_admin_here]

@="在此处打开PowerShell（管理员）"
"Icon"="powershell.exe"

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Directory\background\shell\powershell_admin_here\command]

@="\"C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe\" Start-Process PowerShell -verb RunAs"

; 添加到文件夹右键菜单

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Directory\shell\powershell_admin_here]

@="在此处打开PowerShell（管理员）"
"Icon"="powershell.exe"

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Directory\shell\powershell_admin_here\command]

@="\"C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe\" Start-Process PowerShell -verb RunAs"
```

### 步骤

1. 复制脚本到记事本或notepad++等编辑器上

2. 保存为.reg格式的注册表文件，文件编码选择`UTF-16LE（Little Endian）`，也就是选择`UTF-16LE带签名`，如果选择utf-8会导致中文乱码

3. 运行刚刚保存好的注册表文件，提示确定添加时，点击是

OK，大功告成

管理员身份运行时，会先弹出一个powershell框，然后再弹出授权，授权之后才会出现管理员身份的powershell。暂时没有找到可与去除第一个弹框的办法。

> 此方式添加完毕后，如果在桌面回收站右键显示在此处打开命令行，请打开注册表编辑器，删除此位置`计算机\HKEY_CLASSES_ROOT\Folder\shell`下的`cmd_here`或`powershell_here`文件夹即可