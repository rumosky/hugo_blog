---
title: "PotPlayer实时字幕翻译合集"
slug: "72"
date: 2020-11-21T23:30:00+08:00
lastmod: 2020-11-21T23:30:00+08:00
categories: ["随笔"]
tags: ["potplayer", "翻译"]
draft: false
---

## 说明

最近才发现`PotPlayer`自带字幕实时翻译功能，看一些没有翻译的视频的时候还是比较好用的，众所周知，由于一些国内的原因，PotPlayer内置的微软翻译、谷歌翻译等都不能很方便的使用，所以找了一下国内能用的翻译，特此记录。

本文使用的PotPlayer版本如下：

```bash
版本：201021(1.7.21311)
官网：http://potplayer.daum.net
```

> 这里附上全部的翻译插件合集：[PotPlayer实时字幕翻译合集](https://www.aliyundrive.com/s/yyLgFvjaEJW)

总共目前可用的翻译有三种：

1.百度翻译：源码地址：[PotPlayer_Subtitle_Translate_Baidu](https://github.com/fjqingyou/PotPlayer_Subtitle_Translate_Baidu)
2.腾讯云翻译：源码地址：[subtitle-translate-tmt](https://github.com/BlackGlory/subtitle-translate-tmt)
3.云译翻译：源码地址：[SubtitleTranslate-cloudTranslation](https://github.com/ShererInc/SubtitleTranslate-cloudTranslation)

### 百度翻译

下载后请将`SubtitleTranslate - baidu.as`和`SubtitleTranslate - baidu.ico`这两个文件复制到`C:\Program Files\DAUM\PotPlayer\Extension\Subtitle\Translate`这个路径下，此路径为默认安装路径，请根据自身安装情况自行修改。

打开`PotPlayer`，依次点击`菜单 -> 字幕 -> 实时字幕翻译 -> 实时字幕翻译设置 -> 腾讯机器翻译 -> 帐户设置`，填写你的 `APP ID`和`密钥`，然后配置`PotPlayer`关于实时字幕翻译设置的其他选项, 播放带有字幕文本的视频,测试效果。

> APPID和密钥需要注册成为百度翻译的开发者，请自行百度。

![实测百度翻译效果](https://cdn.rumosky.com/usr/uploads/2020/11/102347813-1.png)

#### 额度

自2022年8月1日起，通用翻译API标准版免费调用量调整为5万字符/月，高级版免费调用量调整为100万字符/月

标准版：
每月前5万字符免费，超出仅收取超出部分费用（QPS=1），按49元/百万字符计费；

高级版：
每月前100万字符免费，超出仅收取超出部分费用（QPS=10），按49元/百万字符计费；

尊享版：
每月前200万字符免费，超出后仅收取超出部分费用（QPS=100），按49元/百万字符计费；

### 腾讯云翻译

源码：

下载后请将`SubtitleTranslate - tmt.as`和`SubtitleTranslate - tmt.ico`这两个文件复制到`C:\Program Files\DAUM\PotPlayer\Extension\Subtitle\Translate`这个路径下，此路径为默认安装路径，请根据自身安装情况自行修改。

打开`PotPlayer`，依次点击`菜单 -> 字幕 -> 实时字幕翻译 -> 实时字幕翻译设置 -> 腾讯机器翻译 -> 帐户设置`，填写你的 `SecretId`和`SecretKey`，然后配置`PotPlayer`关于实时字幕翻译设置的其他选项, 播放带有字幕文本的视频,测试效果。

> 腾讯翻译开发者较麻烦，请按照下述步骤进行

```bash
# 开通腾讯云 机器翻译API(需实名认证)
https://console.cloud.tencent.com/tmt

# 创建腾讯云 SecretId & SecretKey
https://console.cloud.tencent.com/cam/capi
```

![实测腾讯翻译效果](https://cdn.rumosky.com/usr/uploads/2020/11/3055139876.png)

#### 额度

免费500万字符/月，超出后58元/百万字符

### 云译翻译

这个翻译是厦门大学自然语言处理实验室的一个项目，不需要申请ID和key，直接免费使用，无额度限制，翻译效果个人觉得比百度好，其他的请自行测试。

### AWS翻译

这里附上源码地址：https://github.com/mengze-han/potplayer_translate_plug_in

刚刚找到的，我没有测试，自行测试使用。

### 其他

这里为了对比，将其他几个翻译的额度做统计，结果如下：

**阿里云**：免费100万字符/月，超出后50元/百万字符
**有道翻译**：新用户送50元体验金，而100万字符扣48元，也就是累计免费100万字符，超出后需要购买（非专业用途不推荐）
**彩云小译**：免费100万字符/月，超出后20元/百万字符

#### 注意

目前很多网上的视频，包括电影天堂等网站，字幕都是内嵌到帧上面，也就是硬绘制的字幕，这样会导致`PotPlayer`无法读取字幕文本，所以会无法翻译，没有作用，但此时在设置里面是可以测试成功的。这种情况请更换视频，找可以读取字幕文本的视频来进行测试，一般都是外挂字幕可以正常翻译，部分内嵌字幕也可以。

##### 字幕能否翻译的判断方法

在播放的视频界面直接`鼠标右键 -> 字幕 -> 选择字幕`这里如果显示`xxx.ass`或`xxx.ssa`等其他格式的字幕文件，即说明此视频是外挂字幕或内嵌，可以进行翻译，但如果显示`添加字幕...`、`添加次字幕...`、`依次选择字幕`这三个选项，而没有字幕文件，则说明此视频是硬绘制的字幕，无法进行翻译。

## 结语

经过最近一段时间的使用，发现机器翻译的准确度差强人意，其中谷歌和Bing翻译还不错，百度和腾讯有待提高。 并且，实时翻译功能也时好时坏。

经测试，观看雷神4和神秘海域两部电影，共计字符翻译2808次，翻译88832字符，也就是一部电影最多5万字符，所以，百度翻译慎用，重度用户请使用腾讯云翻译或云译翻译（推荐）