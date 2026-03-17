---
title: "OLAINDEX 6.0安装500/401错误解决方案"
slug: "86"
date: 2021-04-07T15:38:00+08:00
lastmod: 2021-04-07T15:38:00+08:00
categories: ["技术"]
tags: ["olaindex", "onedrive"]
draft: false
---

## 引言

最近在使用OLAINDEX的时候遇到了500/401错误，如下图，找了很多方法，最终解决

![401错误](https://cdn.rumosky.com/usr/uploads/2021/04/112494310f.png)

有时不会显示401错误，会直接显示下图，其实也是401错误

![401错误图2](https://cdn.rumosky.com/usr/uploads/2021/04/113539083.png)

500错误会提示`500|服务器错误`，其中`500|Undefined offset:1`是后台配置错误，不是账号问题

### 步骤

问题需要排查，分为宝塔和OneDrive账号两部分，先附上安装教程：[bspost cid="87"]

#### 宝塔面板配置

确保以下内容正确

1. 程序运行目录正确，关闭防跨站攻击
2. 正确配置SSL，可以正常https访问
3. 伪静态规则配置为Laravel 5
4. 配置文件**删除**以下内容：（注意是**删除**不是**注释**）

```bash
location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
    {
        expires      30d;
        error_log /dev/null;
        access_log /dev/null;
    }

    location ~ .*\.(js|css)?$
    {
        expires      12h;
        error_log /dev/null;
        access_log /dev/null; 
    }
```

#### OneDrive账号

首先，登录网页版OneDrive，查看网盘是否可以正常访问和使用，

[教育版OneDrive登录入口](https://www.office.com/?auth=2 "教育版OneDrive登录入口")

账号正常则可能为权限问题，

[国际版账号Azure入口](https://portal.azure.com/ "国际版账号Azure入口")，[世纪互联账号Azure入口](https://portal.partner.microsoftonline.cn/ "世纪互联账号Azure入口")

登录账号之后，进入`Azure Active Directory`控制台，在左侧栏找到`应用注册`，然后在`拥有的应用程序`列表里找到安装OLAINDEX时注册的应用，默认名称为OLAINDEX

在左侧栏`API权限`中，添加下图中的权限

![添加API权限](https://cdn.rumosky.com/usr/uploads/2021/04/542354325.png)

配置好之后，重新在网站后台绑定即可

> 找不到应用或者之前在OLAINDEX上无法创建程序，可以在Azure控制台创建

##### 创建应用

登录账号之后，进入`Azure Active Directory`控制台，在左侧栏找到`应用注册`，点击`新注册`，如下图，名称随意，重定向URL填写回调地址

![注册应用程序](https://cdn.rumosky.com/usr/uploads/2021/04/23124125.png)

左侧栏`证书和密码`，在`客户端密码`中点击`新客户端密码`，会生成`client_id`和`client_secret`

最后添加`API权限`，此处不再赘述。

### 其他

按照本文流程一般都可以解决401/500问题，但如果一切步骤都没有问题，最后还是显示401错误，请更换账号，解释一下，自己注册的教育账号都是子账号，学校edu域管理员的账号权限最高，管理员没有开通相应的权限就无法使用OLAINDEX。如果不更换账号多安装几次，有时会提示`您没有权限，请联系域管理员`

**总结**：

 权限都给了，仍然401，账号管理员开通相应权限，请更换其他域名的教育邮箱 绑定成功，但是首页无内容或者显示500，后台打开账号详情无数据，则是账号已经凉了，这种账号多半是淘宝便宜买的，请更换账号 世纪互联账号基本上不会出问题，但费用较高