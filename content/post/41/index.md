---
title: "宝塔面板安装OLAINDEX 4.0记录"
slug: "41"
date: 2019-09-12T22:55:00+08:00
lastmod: 2019-09-12T22:55:00+08:00
categories: ["技术"]
tags: ["linux", "Windows", "olaindex", "onedrive"]
draft: false
---

## OLAINDEX简介

一款 OneDrive 目录文件索引应用，基于优雅的 PHP 框架 Laravel5.8 搭建，并通过 Microsoft Graph 接口获取数据展示，支持多类型帐号登录，多种主题显示，简单而强大。

源码地址：[OLAINDEX](https://github.com/WangNingkai/OLAINDEX)

> 此文为4.0版本，6.0版本请参考：[bspost cid="87"]

### 安装准备

请自行安装宝塔面板，参考：[宝塔面板安装](https://www.bt.cn/bbs/thread-19376-1-1.html "宝塔面板安装")

安装完宝塔面板之后，请安装好LNMP环境

#### 环境要求

**PHP 扩展要求**

> PHP >= 7.1.3
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

> 在宝塔面板PHP设置里面，删除禁用的`proc_open`，`proc_get_status`，`exec`，`shell_exec`四个函数

**切换composer源**

> 在宝塔shell里执行`composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/`切换为国内源（宝塔面板默认安装了composer）

### 安装开始

1.新建一个站点，不需要创建数据库

![新建站点](https://cdn.rumosky.com/usr/uploads/2019/09/1494653641.png)

2.打开ssh，使用宝塔命令行或者xshell，进入到刚刚新建的站点目录，执行以下命令：

```bash
git clone https://github.com/WangNingkai/OLAINDEX.git tmp 
mv tmp/.git . 
rm -rf tmp 
git reset --hard 
cp database/database.sample.sqlite database/database.sqlite  # 数据库文件
composer install -vvv # 这里确保已经安装composer成功  # 如果报权限问题，建议先执行权限命令
chmod -R 777 storage/
chown -R www:www *
php artisan od:install
```

执行完之后首先绑定网站地址（请输入https地址），设置完后显示默认账户与密码

![配置程序](https://cdn.rumosky.com/usr/uploads/2019/09/3020106898.png)

3.打开宝塔面板，找到刚刚新建的站点，点击设置

4.点击网站目录：勾选取消`防跨站攻击(open_basedir)`，将站点的运行目录改为`public`，别忘了保存

5.点击伪静态，选择`Laravel 5`，保存

6.点击配置文件，注释以下内容。

![注释内容](https://cdn.rumosky.com/usr/uploads/2019/09/3858962882.png)

7.配置SSL（此程序必须https访问）此处不再赘述，自行配置ssl证书即可

8.访问站点，显示登陆页面

9.配置OneDrive账号信息，点击申请，申请后可获得client_id与client_secret，点击保存

![配置OneDrive账号信息](https://cdn.rumosky.com/usr/uploads/2019/09/4263180757.png)

10.点击绑定

![绑定](https://cdn.rumosky.com/usr/uploads/2019/09/3087845104.png)

11.登录OneDrive账号，点击接受

![接受](https://cdn.rumosky.com/usr/uploads/2019/09/989405127.png)

12.进入站点

> 至此大功告成！