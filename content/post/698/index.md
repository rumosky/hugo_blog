---
title: "typecho原生微信小程序"
slug: "698"
date: 2023-07-18T23:38:00+08:00
lastmod: 2023-12-29T00:16:51+08:00
categories: ["技术"]
tags: ["typecho", "微信小程序"]
draft: false
---

## 说明

一直想把网站做成小程序，github上typecho的小程序也都是远古版本，今年官方也升级到了typecho 1.2.1版本，目前还没有可用的小程序，我自己抽空写了一个，开源给大家使用。

先附上开源地址：[Gitee国内](https://gitee.com/rumosky/mp-Blog) [Github](https://github.com/rumosky/mpBlog)

其中，`Restful`后端接口插件，`mp`为小程序源码，`img`为介绍图片

## 小白教程

鉴于很多人询问如何使用，这里先附上一个小白教程：

> 1，下载源码，将`Restful`文件夹内容放在typecho博客程序的`plugin`文件夹下，然后进入博客后台，启用插件
> 2，进入微信小程序后台，点击左侧菜单开发管理，点击顶部菜单开发设置，服务器域名这里添加`request`域名为`https://xxx`，这个域名就是你博客程序的域名
> 3，用微信开发者工具，新建一个微信小程序，目录选择源码的mp目录，修改代码根目录`app.js`里的`baseURL`为你自己的博客地址，然后在当前目录执行npm install，点击工具，构建npm即可
> 4，预览没问题即可上传代码，然后在微信后台提交审核，等待通过就可以访问了。

**注**：如果你启动插件修改了插件的后台请求地址，那么`baseURL`就写你修改后的地址

### 环境

本站环境如下：

```bash
# PHP版本：8.1.17
# 网站服务器：nginx/1.22.1
# 数据库：Pdo_Mysql[Version：5.7.42-log]
# Typecho版本：1.2.1
```

微信小程序没有使用uni-app等框架，采用原生小程序和Vant前端框架，本小程序秉承简洁原则，大方美观

小程序二维码见本站右侧顶部，或扫一扫下图：

![如默星空小程序](https://cdn.rumosky.com/assets/img/mpQRcode.png)

#### 主要功能及优点

* 首页文章列表、文章搜索、文章分类浏览、文章归档时间轴、关于

* 浏览文章评论、分享页面、复制联系信息

* 文章代码高亮、行号、语言显示

* 文章链接自动复制

* 文章表格横向滚动

* 一键返回顶部

* 接口请求时间限制，防止崩溃

* 使用缓存，提升加载速度

#### 预览图

![首页](https://cdn.rumosky.com/usr/uploads/2023/07/4023559299.png)

![分类](https://cdn.rumosky.com/usr/uploads/2023/07/759106598.png)

![归档](https://cdn.rumosky.com/usr/uploads/2023/07/1575330300.png)

![关于](https://cdn.rumosky.com/usr/uploads/2023/07/1816346630.png)

![代码高亮](https://cdn.rumosky.com/usr/uploads/2023/07/489223255.png)

![评论](https://cdn.rumosky.com/usr/uploads/2023/07/1951310931.png)

#### 后端

代码中`typecho-plugin-Restful`文件夹为接口插件，修改名称为Restful，上传到插件目录，启用即可。

默认后端请求地址为`https://xxx.com/api/`，可自行在插件中设置，此插件源码地址为：[typecho-plugin-Restful](https://github.com/moefront/typecho-plugin-Restful)

#### 前端

小程序代码，appjs里修改后端接口地址：

```javascript
// app.js
App({
  onLaunch() {
    // 获取当前年份
    const date = new Date();
    const currentYear = date.getFullYear();
    this.globalData.currentYear = currentYear;
  },

  globalData: {
    baseURL: "https://xxx",
    currentYear: null,
  },
});
```

这个接口地址需要在微信小程序开发后台添加一下，然后执行`npm install`，然后构建npm即可

#### 已知问题

* 评论功能暂不支持，目前也不打算支持，因为不简洁而且不容易过审

#### 计划更新

1. 增加黑暗模式（功能不复杂，开发版已支持，就是实在懒得适配css，很烦）
2. 自己写接口，就当练习了
3. 未完待续……

#### 小程序审核

审核之前，后台最好备份一下数据，然后留两三篇技术文章，不要抄袭，不要发资讯之类的，一般都没问题，评论最好不要有。

本站小程序从提交审核到通过，14分钟，很快，如下图：

![微信小程序审核](https://cdn.rumosky.com/usr/uploads/2023/07/3747044093.png)

### 结语

这个微信小程序的本意就是方便在移动端浏览文章，所以阅读是最重要的功能，其余都是点缀，不希望太复杂，虽然网站是响应式布局，但很少有人用手机访问。

BUG反馈请提交issue，留言请在本文留言，欢迎提出意见，开源程序，随缘更新。