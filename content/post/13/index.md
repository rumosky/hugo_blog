---
title: "mindoc接口文档在线管理系统进阶配置"
slug: "13"
date: 2019-01-12T10:35:00+08:00
lastmod: 2019-01-12T10:35:00+08:00
categories: ["技术"]
tags: ["宝塔面板", "nginx", "mindoc"]
draft: false
---

## 前言

mindoc文档管理系统是一款国产的轻量级的Wiki系统，为了有更好的体验，需要配置一下。

mindoc程序安装请参考：[bspost cid="12"]

安装好之后就可以尽情使用了，但是，这里还有一点小技巧可以增强体验感。

### 邮件配置

> 配置好邮件之后，用户忘记账号密码就可以通过发送邮件来重置密码，很便捷。

#### 参考配置

```bash
####################邮件配置######################
# 是否启用邮件
enable_mail=true
# 每小时限制指定邮箱邮件发送次数
mail_number=20
# smtp服务用户名
smtp_user_name=xxx@xxx.com
# smtp服务器地址
smtp_host=smtp.xxx.com
# smtp密码
smtp_password=xxxxxxx
# 端口号
smtp_port=465     //一般加密端口都是465，未加密是25，详情请咨询邮箱服务商
# 发送邮件的显示名称
form_user_name=xxxx
# 邮件有效期30分钟
mail_expired=30
# 加密类型NONE 无认证、SSL 加密、LOGIN 普通用户登录
secure=SSL
```

##### 说明

各大邮箱基本上都支持，按照说明开启就行了。唯一注意的就是，邮箱使用的是SMTP发信，QQ邮箱等其他邮箱请先在设置里面开启SMTP发信（pop3是另一种，mindoc不支持）

### 配置CDN

> 这里说的CDN是回源CDN

- 其实就是将mindoc程序内部的静态js,css,images资源放在CDN服务器上，这样加载速度就会很快。


#### 参考配置

```bash
###############配置CDN加速##################
cdn=https://cdn.rumosky.com/mindoc/
cdnjs=https://cdn.rumosky.com/mindoc/
cdncss=https://cdn.rumosky.com/mindoc/
cdnimg=../
```

##### 说明

1.CDN资源地址请填写资源目录的上一级，比如，mindoc默认的静态资源在根目录下的static文件夹里，需要将static文件夹全部放在CDN服务器上，路径填写你存放static文件夹的那个目录就行了。程序会自动查找静态资源。
2.不能直接写`https://cdn.xxx.com/static/`
3.因为static里面都包含了js,css,images，所以四个配置地址填相同的即可
4.图片非常少，所以博主img就没有配置CDN地址，调用的是网站服务器的内的文件。


### 开启Gzip压缩

> 开启这个压缩之后速度明显有提升，也会降低页面大小和流量。

#### 参考配置

```bash
# 开启压缩
EnableGzip=true

# 压缩级别,取值为 1~9,如果不设置为 1(最快压缩)
gzipCompressLevel = 9

# 压缩长度阈值, 当原始内容长度大于此阈值时才开启压缩,默认为 20B(ngnix默认长度)
gzipMinLength = 256

# 请求类型,针对哪些请求类型进行压缩,默认只针对 GET 请求压缩
includedMethods = get;post
```

有的时候配置文件里面没有开启压缩这一段代码，请不要着急，直接将上述配置复制到配置文件的最后面即可。

### 总结

- 配置邮箱真的很有必要，尤其是多人使用文档系统的时候。
- CDN静态资源迁移之后，访问速度明显快了很多。没分离静态文件的速度和现在不能比！
- 压缩页面也能提升速度。
- 还可以开启缓存，开启Redis等，具体请仔细研究配置文件。