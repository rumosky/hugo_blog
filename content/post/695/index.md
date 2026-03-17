---
title: "微信小程序使用fontawesome4.7图标方法"
slug: "695"
date: 2023-07-18T22:21:00+08:00
lastmod: 2023-07-18T22:21:00+08:00
categories: ["随笔"]
tags: ["微信小程序", "fontawesome"]
draft: false
---

## 说明

最近在写了一个小程序，原生组件里面的图标很少，而且不好看，这里记录一下如何使用fontawesome4.7图标的方法。

### 步骤

这里说一下为啥不用v5或者v6版本，因为新版太多了，用不到那么多，v4.7都已经很多了，最主要的是，新版需要js支持，使用的时候需要安装很多依赖，网上说直接转换的那种方式，效果不太好，有的也用不了，4.7版本稳定且只需要css即可

**懒人看这里**：下载地址：[fontawesome4.7](https://rumosky.lanzouk.com/izMv412usvwf) 密码:h201

下载好之后直接放在小程序目录，在app.wxss文件里面引用即可，如下：

```css
@import "assets/fontawesome4.7/fontawesome.wxss";
@import "assets/fontawesome4.7/stylesheet.wxss";
```

这样就可以直接使用了

#### 自行配置

首先，附上fontawesome4.7图标官方地址：[fontawesome](https://fontawesome.com/v4/get-started/)

这里附上下载地址，[fontawesome4.7](http://fontawesome.dashgame.com/assets/font-awesome-4.7.0.zip)

1，下载解压之后，找到`fontawesome-webfont.ttf`

2，进入以下网站进行格式转换，地址：[transfonter](https://transfonter.org/)，打开Base64 encode，Formats选项中的字体全部勾选，点击add fonts，选择刚刚下载的`fontawesome-webfont.ttf`，点击convert即可

3，转换之后的文件中，找到`stylesheet.css`文件，修改后缀名为`wxss`，得到`stylesheet.wxss`

4，下载的fontawesome4.7中css文件夹里`font-awesome.css`文件，复制这个文件里除了`@font-face{}`之外的其余全部内容，放在一个新的文件里，假设叫`font-awesome.wxss`文件

5，将3、4步得到的两个文件，放在小程序文件夹中，在app.wxss文件里面引用即可，如下：

```css
@import "assets/fontawesome4.7/fontawesome.wxss";
@import "assets/fontawesome4.7/stylesheet.wxss";
```

### 最后

使用的时候如下：

```html
<text class="fa fa-user"></text>
```

这样就可以了