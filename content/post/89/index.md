---
title: "WordPress配置自定义邮件回复（模板+内容）"
slug: "89"
date: 2021-09-10T15:57:00+08:00
lastmod: 2021-09-10T15:57:00+08:00
categories: ["杂谈"]
tags: ["WordPress"]
draft: false
---

## 说明

之前刚开始使用Wordpress，不知道用哪一款邮件插件，试了很多，最后选择了 WP SMTP 和 Better Notifications for WordPress

### 使用

邮件通知使用的是 WP SMTP，这个插件的作用是配置邮件通知，在后台写入SMTP地址，端口号，账户和密码即可，国内的QQ和163都是需要授权码的。这个插件配置完成之后，只需要测试发送即可，收到邮件就证明一切正常。

但是，用了上述插件，邮件发送的内容是默认的，样式什么的，也都是固定的，如果要修改，需要修改源码，使用插件的话更方便，推荐使用 [Better Notifications for WordPress](https://wordpress.org/plugins/bnfw/) 

这个插件的作用是配置相应的发送规则，可以自定义发送内容，主要有这几个维度：

> WordPress 默认通知
> 页面
> 文章

在设置的时候，可以选择新评论通知，审核通过通知，规则逻辑一定要清楚，建议不要直接默认通过评论。

## 结语

由于使用WordPress的时候没有写这篇文章，很多细节都忘记了，之前的素材也没了，但是只要逻辑清晰，花时间配置一下还是很好用的