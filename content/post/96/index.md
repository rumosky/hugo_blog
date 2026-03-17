---
title: "Windows 11正式版发布，官方原版ISO下载以及使用体验"
slug: "96"
date: 2021-10-07T16:22:00+08:00
lastmod: 2021-10-07T16:22:00+08:00
categories: ["杂谈"]
tags: ["Windows", "win11", "微软"]
draft: false
---

## 说明

今天微软在官网公开了Win11的下载，正式版（RTM）版本号敲定为22000.194，之前已经安装的用户没有必要重新安装，因为之前RP通道提供的候选版本的SHA1值与今天发布的RTM版一样，重命名而已。微软称，将在后续继续发布多个更新，修复目前版本存在的问题。

今天开始微软将陆续为满足条件的计算机推送Win11 RTM版在线升级，也就是说即便你的计算机满足升级条件，也不一定立刻收到推送，推送是分批进行的，据悉到明年这个时候，微软才会完成全部推送。

Win10 2004及以上版本可以直接在线升级为Win11，且升级是免费的，只要原Win10已激活，那么升级后Win11也是激活的，不需要额外付费。目前微软仅发布64位版本，32位版本好像被微软的砍刀部砍掉了，或许从Win11开始就再也没有32位系统了。

### 版本说明

关于RTM版、MSDN版、MVS版、VLSC版的含义，很多人在问，在下面详细解释一下：

> Windows10不同版本的区别：
参考官方说明： [比较 Windows 10 的不同版本](https://www.microsoft.com/zh-cn/windowsforbusiness/compare)
其中功能最强的是企业版，其次是教育版。
MSDN、VLSC、RTM版本的区别：
关于版本的说法，MSDN、VLSC、RTM等好多人搞不清楚，在这重新介绍一下。其中RTM(Release To Manufacturing)是发布给制造商的版本，也就是正式版的意思。
MSDN和VLSC实际上不是版本，而是两个发布渠道：
1、Microsoft Developer Network (MSDN)，为开发者网络渠道，现在微软将MSDN的下载转移到了My Visual Studio (MVS) ，发布的版本都是正式版，所以大家通常称之为MSDN版或MVS版，以前只发布零售版，现在零售版和批量版都发布。
2、Volume License Service Center (VLSC)，为批量许可服务中心，发布的版本是正式版，大家通常称之为VOL版或VL版，是批量授权版。


简而言之，RTM是正式版的统称。MVS和VLSC是两个发布渠道，MSV以发布零售版为主，VLSC只发布批量版本，由于发布的都是正式版，所以大家也称之为MVS版或VLSC版。MVS就是原先的MSDN，虽然现在改名了，但是大家还是习惯称之为MSDN。

所以，以上说法都是正式版，只不过不同渠道发布的版本略有区别。多年前微软是不公开提供免费下载ISO镜像的，网上的版本都是从MSDN渠道泄漏出来的，而现在消费者版可以在官网直接免费下载。批量版目前还是要有订阅账号（收费），才可以下载。

### 硬件要求

此外，大家最关心的就是硬件问题了，微软给出了最低硬件条件的限制：

> 1、TPM2.0；
2、4GB内存；
3、64GB硬盘；
4、支持UEFI安全启动；
5、1GHz及以上的64位CPU

下图为微软官网最低系统要求：

![Windows 11最低系统要求](https://cdn.rumosky.com/usr/uploads/2021/10/QQ截图20211005204247.png)

这个配置要求中，很多老电脑是没有TPM2.0的，但这个根本不是影响系统流畅运行的因素。支持UEFI安全启动来说，2013年及其以后的电脑基本都支持，但它也不是影响流畅运行的因素。剩余的硬件条件，其实要求已经很低了，几乎很少有不满足这个要求的。CPU注意需要64位架构的，硬件配置要求并不算高，只要能流畅运行Win10(64位)的机器，我认为流畅运行Win11没有任何问题，反而可能会更流畅一些。这个最低硬件要求，和Win7比起来，都差不太多，所以，硬件的问题不用过于担心。

不过，还是展示一下微软给出的警告： 不建议在不符合 Windows 11 最低系统要求的电脑上安装 Windows 11 媒体，这可能会导致兼容性问题。如果您继续在不符合要求的电脑上安装 Windows 11，则该电脑将不再受支持，也无权接收更新。由于缺乏兼容性而导致的电脑损坏不在制造商的保修范围内。

最后附上官方的一个教程： [在电脑上启用TPM2.0](https://support.microsoft.com/zh-cn/windows/%E5%9C%A8%E7%94%B5%E8%84%91%E4%B8%8A%E5%90%AF%E7%94%A8-tpm-2-0-1fd5a332-360d-4f46-a1e7-ae6b0c90645c#bkmk_enable_tpm)

### 原版镜像下载

Win11主要包含两大版本：消费者版、商业版。消费者版可前往微软官网下载： [Download Windows 11](https://www.microsoft.com/zh-cn/software-download/windows11)

#### 步骤

进入上面的链接之后，如下图：

![Windows 11下载页面](https://cdn.rumosky.com/usr/uploads/2021/10/网页捕获_5-10-2021_211038_www.microsoft.com_.jpeg)

官方提供了三种方式安装，第一种就是使用安装助手无脑下一步，第二就是利用官方工具下载制作镜像，第三种就是直接下载镜像文件。

也可以直接使用下方链接下载（提取自官网）：
1， [Windows 11安装助手](https://download.microsoft.com/download/3/a/9/3a9e2fe1-96e7-4514-8744-f3a9731f91c7/Windows11InstallationAssistant.exe)
2， [Windows 11安装媒体](https://software-download.microsoft.com/download/pr/888969d5-f34g-4e03-ac9d-1f9786c69161/MediaCreationToolW11.exe)
3， [Windows 11磁盘映像（ISO）](https://software-download.microsoft.com/sg/Win11_Chinese(Simplified)_x64.iso?t=fda31835-72e0-4ada-9d04-bf008bd83eca&e=1633516400&h=a9305e87fb0079e9d2ab10fa790f56ef)

在安装之前建议下载微软官方的电脑健康状况检查工具检测一下，官方链接： [电脑健康状况检查](https://www.microsoft.com/en-us/windows/windows-11#pchealthcheck)，也可以点击这里直接下载： [电脑健康状况检查](https://download.microsoft.com/download/5/b/4/5b4103b3-3135-4958-987d-3aba103333f6/3.1/X64/WindowsPCHealthCheckSetup.msi)

我的台式机就不支持，因为CPU的原因，如下图：

![电脑状况检查](https://cdn.rumosky.com/usr/uploads/2021/10/QQ截图20211005220702.png)

> AMD处理器锐龙从二代开始才可以升级，一代锐龙不支持，详细CPU支持列表请访问： [Windows处理器要求](https://docs.microsoft.com/zh-cn/windows-hardware/design/minimum/windows-processor-requirements)

但是我的笔记本支持！

![电脑健康检查通过](https://cdn.rumosky.com/usr/uploads/2021/10/1.png)

#### 使用体验

说实话，因为笔记本是8G的内存，以前的本子，换了Windows 11之后感觉确实快一些，流畅很多，说明是有优化提升的。但其余的功能怎么说呢，还行吧，没有网上看到的那么不方便，可能我都用不到？整体来说，还是进步的，体验不错，新鲜感过了之后也就是一个工具罢了。虽然是正式版，但好像还是不稳定，不过也正常，毕竟才刚刚发布，期待之后的更新吧。

**2021.10.07补充更新**

今天刚刚给台式机升级成功，补充一些图和详细内容，不兼容机器请参考： [bspost cid="95"]

首先最重要的就是主页面的修改，任务栏默认居中（可以修改为左侧）：

![Windows 11桌面](https://cdn.rumosky.com/usr/uploads/2021/10/QQ图片20211007135339.png)

新的开始菜单，去除了磁贴：

![Windows 11开始菜单](https://cdn.rumosky.com/usr/uploads/2021/10/QQ截图20211007151705.png)

文件资源管理器：

![Windows 11文件资源管理器](https://cdn.rumosky.com/usr/uploads/2021/10/QQ截图20211005220702-1.png)

新的设置界面：

![Windows 11设置](https://cdn.rumosky.com/usr/uploads/2021/10/QQ截图20211007144324.png)

新的输入法皮肤（个人比较喜欢这个）：

![Windows 11输入法](https://cdn.rumosky.com/usr/uploads/2021/10/微信图片_20211007135409.png)

##### 总结

1. 外观变化如上图，整体图标全部重新设计，新音效，新动画，窗口圆角化，但目前动画只支持原生应用。

2. 速度上面确实比Windows10快很多，尤其是开机，之前我这个电脑开机最快22秒，最慢32秒，现在开机最快一次6秒，最慢15秒。

3. 其余的没有感觉和 Win10 有太大的区别，可能是因为还用不到吧。总之，整体比较满意。