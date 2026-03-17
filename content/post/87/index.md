---
title: "宝塔面板安装OLAINDEX 6.0教程"
slug: "87"
date: 2021-04-08T15:51:00+08:00
lastmod: 2021-04-08T15:51:00+08:00
categories: ["技术"]
tags: ["olaindex", "onedrive"]
draft: false
---

## 引言

之前使用过OLAINDEX这个程序，当时还是4.0，现在已经更新到6.0版本，安装的过程和之前有一些变化，所以均为新版安装不同之处的文字记录，无图，4.0版本请参考：[bspost cid="41"]

### 步骤

 [OLAINDEX源码](https://github.com/WangNingkai/OLAINDEX)

默认已经安装好宝塔面板（宝塔面板安装请参考： [宝塔面板安装](https://www.bt.cn/bbs/thread-19376-1-1.html)

安装完宝塔面板之后，请安装好LNMP环境

#### 环境要求

**PHP 扩展要求**

> PHP >= 7.4
> PHP OpenSSL 扩展
> PHP PDO 扩展
> PHP Mbstring 扩展
> PHP Tokenizer 扩展
> PHP XML 扩展
> PHP Ctype 扩展
> PHP JSON 扩展
> PHP BCMath 扩展
> PHP Fileinfo 扩展


**禁用函数**

在宝塔面板PHP设置里面，删除禁用的`proc_open`，`proc_get_status`，`putenv`三个函数

宝塔面板默认composer源为阿里云，无需修改。

### 安装开始


1.新建一个站点，不需要创建数据库，绑定一个域名即可

2.打开ssh，使用宝塔命令行或者xshell，进入到刚刚新建的站点目录，执行以下命令：

```bash
git clone https://github.com/WangNingkai/OLAINDEX.git tmp
mv tmp/.git .
rm -rf tmp
git reset --hard
composer install -vvv
chmod -R 777 storage
chown -R www:www * # 此处 www 根据服务器具体用户组而定
composer run install-app (此为自动安装，默认sqlite存储数据)
```

安装成功后，会显示默认后台账户和密码

3.打开宝塔面板，找到刚刚新建的站点，点击设置

4.点击网站目录：勾选取消`防跨站攻击(open_basedir)`，将站点的运行目录改为`public`，别忘了保存

5.点击伪静态，选择`Laravel 5`，保存

6.点击配置文件，注释以下内容。

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


7.配置SSL（此程序必须https访问）此处不再赘述，自行配置ssl证书即可

8.访问站点，显示登陆页面

9.配置OneDrive账号信息，点击申请，申请后可获得`client_id`与`client_secret`，在相应位置填写即可，注意账号的类别，国际版和世纪互联版本。

10.登录OneDrive账号，此处微软会提示请求证得许可，点击接受即可

![接受](https://cdn.rumosky.com/usr/uploads/2019/09/989405127.png)

11.绑定成功之后，返回网站首页，会默认显示网盘内容，后台也会显示账号详情，如果账号详情内无法显示账号容量等信息或首页无数据，则绑定失败。

### 错误处理

若绑定之后显示401/500错误，请参考：[bspost cid="86"]