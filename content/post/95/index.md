---
title: "Windows 11系统最新升级攻略（不兼容硬件也可升级）"
slug: "95"
date: 2021-10-08T16:21:00+08:00
lastmod: 2021-10-08T16:21:00+08:00
categories: ["杂谈"]
tags: ["Windows", "win11", "微软"]
draft: false
---

## 说明

两天前Windows11正式发布，详细内容请访问：

[bspost cid="96"]

当时我的台式机不支持升级，因为CPU不在支持的名单里，这两天找了一下教程，终于正常安装。

如果只想顺利安装Win11，那么直接看今天的教程

### 步骤

首先要了解，有升级安装和全新安装两种方式：

> 懒癌晚期患者，想保留当前系统中已安装的各类软件、数据等，可采用升级安装的方式。
洁癖强迫症患者，通常采用格式化C盘的方式，全新安装，C盘所有数据均被删除。

两种方式各有利弊，比如当前系统本身就存在各种问题，升级安装后大概率也会出现疑难杂症。全新安装一般不会有各类稀奇古怪的问题，但是要重新再安装一遍常用软件，费时间。

#### Win11升级安装

升级安装也分为两种，直接通过Windows Update在线升级安装；还可以下载原版ISO镜像，离线升级安装。

无论是哪种升级方式，针对硬件不满足条件的用户，把下面的代码另存为`skip.bat`备用。如果硬件满足最低要求，则忽略此节。

```bash
@(set "0=%~f0"^)#) & powershell -nop -c iex([io.file]::ReadAllText($env:0)) & exit/b
#:: double-click to run or just copy-paste into powershell - it's a standalone hybrid script
#:: v2 of the toggle script comes to the aid of outliers for whom v1 did not work due to various reasons (broken/blocked/slow wmi)

$_Paste_in_Powershell = {
  $N = 'Skip TPM Check on Dynamic Update'
  $0 = sp 'HKLM:\SYSTEM\Setup\MoSetup' 'AllowUpgradesWithUnsupportedTPMOrCPU' 1 -type dword -force -ea 0
  $B = gwmi -Class __FilterToConsumerBinding -Namespace 'root\subscription' -Filter "Filter = ""__eventfilter.name='$N'""" -ea 0
  $C = gwmi -Class CommandLineEventConsumer -Namespace 'root\subscription' -Filter "Name='$N'" -ea 0
  $F = gwmi -Class __EventFilter -NameSpace 'root\subscription' -Filter "Name='$N'" -ea 0
  if ($B) { $B | rwmi } ; if ($C) { $C | rwmi } ; if ($F) { $F | rwmi }
  $C = "cmd /q $N (c) AveYo, 2021 /d/x/r>nul (erase /f/s/q %systemdrive%\`$windows.~bt\appraiserres.dll"
  $C+= '&md 11&cd 11&ren vd.exe vdsldr.exe&robocopy "../" "./" "vdsldr.exe"&ren vdsldr.exe vd.exe&start vd -Embedding)&rem;'
  $K = 'HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\vdsldr.exe'
  if (test-path $K) {ri $K -force -ea 0; write-host -fore 0xf -back 0xd "`n $N [REMOVED] run again to install "; timeout /t 5}
  else {$0=ni $K; sp $K Debugger $C -force; write-host -fore 0xf -back 0x2 "`n $N [INSTALLED] run again to remove ";timeout /t 5}
} ; start -verb runas powershell -args "-nop -c & {`n`n$($_Paste_in_Powershell-replace'"','\"')}"
$_Press_Enter
#::
```

##### 在线升级方法

硬件满足条件的用户，直接在Windows Update中点击更新即可。如果硬件不满足条件，双击运行`skip.bat`后（绿色代表安装成功），在Windows Update中，再次手动检查更新，如果出现了推送，则直接下载安装，全过程自动，无需任何操作。

由于微软分批推送Win11，所以短期内很多人可能收不到推送。收不到怎么办？那就只能下载ISO，离线升级。

##### 离线升级方法

1. 下载原版ISO镜像（[Windows 11正式版发布，官方原版ISO下载以及使用体验](https://rumosky.com/archives/91)），进行解压或使用虚拟光驱挂载（Win10中双击即可挂载）；
2. 断开网络连接；
3. 双击运行`skip.bat`（绿色代表安装成功）；
4. 双击ISO根目录下的setup.exe，按提示一直下一步即可。

#### 补充

* 批处理`skip.bat`相当于安装了“监控（删除硬件检测相关的文件并修改注册表）”，使用完成后可以再次双击运行批处理进行卸载（紫色代表卸载完成）。

* 本文给出的批处理，只是其中的一种方法，还有其他的，比如，针对于没有uefi、没有tpm、不支持的cpu有这三者中的任意情况均可使用：1、下载win10 iso；2、下载win11 iso；3、将win10 iso刻录到u盘，提取win11 iso中sources/install.wim 复制到u盘相同位置替换文件。4、使用u盘启动安装，就像安装win10一样，装好就是win11。 如果之前激活过win10，全新安装win11，跳过序列号，完成之后联网也是激活的。按此方法安装，简单，一次成功，还可联网更新。摘自雪龙郎微博

* 本文提供的批处理来自 [mydigitallife](https://forums.mydigitallife.net/threads/windows-11-setup-tpm-bypass-the-many-ways.84063/)

* 以上内容适用于现有系统可以正常进入的情况，如果系统无法进入，有两种方案：1，使用WindowsPE进行安装；2，将上面下载的ISO镜像直接刻录到U盘安装；两种方式都可以，此处就不再赘述。