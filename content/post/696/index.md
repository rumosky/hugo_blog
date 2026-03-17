---
title: "微信小程序mp-html插件使用atom one dark代码高亮"
slug: "696"
date: 2023-07-18T22:45:00+08:00
lastmod: 2023-07-18T22:45:00+08:00
categories: ["随笔"]
tags: ["微信小程序", "mp-html"]
draft: false
---

## 说明

最近写了一个微信小程序，使用了mp-html插件来解析markdown文章，记录一下如何修改代码高亮。

### 步骤

先附上mp-html官网：[mp-html](https://jin-yufeng.gitee.io/mp-html/#/overview/quickstart)

官方文档上其实写了方法，这样写的：

*本插件的高亮功能依赖于 prismjs，默认配置中仅支持 html、css、c-like、javascript 语言和 Tomorrow Night 主题，如果需要更多语言或更换主题请前往 官网 下载对应的 prism.min.js 和 prism.css 并替换 plugins/highlight/ 目录下的文件（prismjs 的插件大多涉及 dom 操作，基本不可用，请勿选择）*

但官方文档没具体写后续怎么操作，而且`prismjs`官方提供的主题太少了，并且很丑，下面记录一下：

1，前往prismjs官方下载页面，[prismjs](https://prismjs.com/download.html)

2，选择你需要的语言高亮，不要选择`Plugins`下的内容，选好之后，点击`download js`，替换`xxx\node_modules\mp-html\plugins\highlight`里的`prism.min.js`，文件名保持一致。

3，前往这里：[prism-themes](https://github.com/PrismJS/prism-themes)，下载`themes`下的`prism-one-dark.css`，用这个替换`xxx\node_modules\mp-html\plugins\highlight`里的`prism.css`，文件名保持一致。

3，**划重点**：切换到`xxx\node_modules\mp-html`这个目录下，执行`npm install && npm run build:weixin`，执行结束之后，在微信开发者工具中，点击顶部菜单`工具`，然后点击`构建npm`

这样，代码高亮就替换了，路径里的xxx是微信小程序文件夹