---
title: "阿里云OSS防盗链设置"
slug: "81"
date: 2021-04-27T15:07:00+08:00
lastmod: 2021-04-27T15:07:00+08:00
categories: ["杂谈"]
tags: ["aliyun", "OSS"]
draft: false
---

## 说明

启用阿里云OSS之后遇到了一些问题，记录一下

### 防盗链设置

进入OSS控制台，选中相应的bucket，权限管理，防盗链设置

默认有两个设置选项：


> referer规则一行写一条，支持通配符*，
> 例如www.124.221.171.144; *.124.221.171.144等 空referer 
> 允许空referer即浏览器地址栏可以直接访问，相反则不允许访问，会提示403 forbidden

以下内容详细介绍如何配置Referer，配置Referer时使用了通配符的具体示例，以及配置Referer后的效果。


##### 配置Referer

- 单个Bucket支持配置多个Referer。通过控制台设置Referer时，使用回车作为换行符分隔；通过API设置Referer时，使用英文逗号（,）分隔。
- Referer参数支持通配符星号（\*）和问号（?）。
- 通配符星号`*`表示使用星号代替0个或多个字符。若Referer白名单配置为`*.aliyun.com`，且不允许空Referer的情况下，则只有HTTP或HTTPS header中包含Referer字段的请求才能访问OSS资源，例如`help.aliyun.com`、`www.aliyun.com`等；若Referer白名单配置为`*.aliyun.com`，且允许空Referer的情况下，则Referer为空的请求也允许访问OSS资源。
- 通配符问号`?`表示使用问号代替一个字符。若Referer白名单配置为`aliyun?.com`，且不允许空Referer的情况下，则只有HTTP或HTTPS header中包含Referer字段的请求才能访问OSS资源，例如`aliyuna.com`、`aliyunb.com`等；若Referer白名单配置为`aliyun?.com`，且允许空Referer的情况下，则Referer为空的请求也允许访问OSS资源。


##### Referer效果

- 如果Referer白名单为空，则所有的请求都会被允许。
- 如果Referer白名单不为空，且不允许Referer字段为空，则只有Referer属于白名单的请求被允许，其他请求（包括Referer为空的请求）会被拒绝。
- 如果Referer白名单不为空，但允许Referer字段为空，则Referer为空的请求和符合白名单的请求会被允许，其他请求都会被拒绝。


#### 注意

OSS官方文档内写的格式是`xxx.com`，其中没有协议头，但亲测，必须加上协议头设置才会成功，否则，会全部提示`403 forbidden`

> 完整的规则应该是`https://cdn.rumosky.com`，后经阿里云工程师确认，确实必须加上协议头。
> 若网站内有空referer请求，建议开启允许空referer，否则会报错