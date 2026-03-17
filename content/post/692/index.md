---
title: "微信小程序使用阿里巴巴矢量图标库方法"
slug: "692"
date: 2023-07-18T21:38:00+08:00
lastmod: 2023-07-18T21:38:00+08:00
categories: ["随笔"]
tags: ["微信小程序", "阿里巴巴矢量图标库"]
draft: false
---

## 说明

最近在写了一个小程序，原生组件里面的图标很少，而且不好看，这里记录一下如何使用阿里巴巴矢量图标库的方法。

### 步骤

先附上阿里巴巴矢量图标库地址：[阿里巴巴矢量图标库](https://www.iconfont.cn/)

1，选择想要的图标，点击添加入库，也就是购物车图标

2，全部添加好之后，点击右上角购物车，选择添加至项目

3，选择font class，点击下载至本地，或者在线链接也可以，复制`iconfont.css`内容到微信小程序代码里，文件名`iconfont.wxss`

5，在`app.wxss`文件里添加`@import "path/to/iconfont.wxss";`

6，使用的时候`class="iconfont icon-github"`这样写就可以了，前缀是iconfont，这个可以在项目设置里面修改，默认是这个

这个方法唯一的问题是需要在微信开发者后台添加一个download域名，`https://at.alicdn.com/`，这样才不会报错，才可以正常引用。

#### 本地方式

如果不想添加`https://at.alicdn.com/`，则需要先添加购物车，选择font class之后，下载下来，将`iconfont.ttf`这个文件上传到这里：[transfonter](https://transfonter.org/)，如下图：

![transfonter](https://cdn.rumosky.com/usr/uploads/2023/07/2643348832.png)

打开Base64 encode，ttf勾选上，上传之后，点击convert，把转换后的文件中，复制`stylesheet.css`全部内容，替换掉`iconfont.css`中的`@font-face{}`

修改`iconfont.css`后缀名为`wxss`，然后在微信小程序中引用这个文件即可，参照上面方法的5、6步。