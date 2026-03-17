---
title: "php7+nginx+mysql配置（wnmp环境）"
slug: "26"
date: 2019-03-22T20:43:00+08:00
lastmod: 2019-03-22T20:43:00+08:00
categories: ["技术"]
tags: ["linux", "Windows", "php", "nginx", "mysql"]
draft: false
---

## 介绍

WNMP环境配置（Windows+Nginx+MySQL+php）

本文环境：

```txt
Windows10 x64 专业版
Nginx1.14.2 稳定版
MySQL 5.7.25
PHP 7.2.16
```

### 下载

相应地址：[Nginx](https://nginx.org/en/download.html "Nginx")，[MySql](https://dev.mysql.com/downloads/ "MySql")，[php](https://php.net/downloads.php "php")

### 安装

本文安装目录如下

```txt
D:/
 └──wnmp
     ├──nginx
     ├──php
     ├──mysql
     └──www  // web文件
```

安装顺序可以随意，没有次序

#### MySQL安装

1.下载好的安装包直接运行，同意协议之后，可以直接默认develop安装（全部默认，建议新手选择），也可以选择custom自定义安装（可以修改安装位置，可以选择安装组件）

2.默认安装完成之后会提示设置密码password，其余一切默认即可

3.安装完成之后默认会启动mysql服务，也会注册为开机启动，以后都可以不用管


#### Nginx安装

1.将下载好的压缩包解压

2.无须安装，直接将文件放在你想放置的位置

3.打开文件夹conf下的nginx.conf文件，用记事本或者notepad++打开

4.找到43-46行的如下代码

```bash
location / {
    root   html;
    index  index.html index.htm;
}
```

修改成下列内容：

```bash
location / {
    root   D:/wnmp/www; //此处是web文件位置
    index  index.html index.htm index.php;
}
```

5.找到65-71行的如下代码

```bash
location ~ \.php$ {
    root           html;
    fastcgi_pass   127.0.0.1:9000;
    fastcgi_index  index.php;
    fastcgi_param  SCRIPT_FILENAME  \scripts$fastcgi_script_name;
    include        fastcgi_params;
}
```

修改成下列内容

```bash
location ~ \.php$ {
    root           D:/wnmp/www;
    fastcgi_pass   127.0.0.1:9000;
    fastcgi_index  index.php;
    fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
    include        fastcgi_params;
}
```

6.双击nginx.exe即可启动Nginx服务

> 其实Nginx配置只需修改root目录为自己的web目录，第二次修改的内容是为了支持PHP，修改地方只有`\scripts`变为`$document_root`

7.在浏览器输入`127.0.0.1`或者`localhost`，出现`welcome to nginx`说明配置成功，若没出现，则请查看任务管理器里是否有Nginx服务

#### PHP安装

1.将下载好的PHP压缩包解压，跟Nginx一样，无须安装，直接放置到自己的环境目录下即可

2.复制`php.ini-development`文件，重命名为`php.ini`

3.用记事本或者notepad++打开`php.ini`文件

4.修改以下内容

- 搜索“extension_dir”，找到：;extension_dir = "ext" 先去前面的分号再改为 extension_dir = "D:/wnmp/php/ext"
- 搜索“date.timezone”，找到：;date.timezone = 先去前面的分号再改为 date.timezone = Asia/Shanghai
- 搜索“enable_dl”，找到：enable_dl = Off 改为 enable\_dl = On
- 搜索“cgi.force_redirect” ;cgi.force_redirect = 1 先去前面的分号再改为 cgi.force_redirect = 0
- 搜索“fastcgi.impersonate”，找到：;fastcgi.impersonate = 1 去掉前面的分号
- 搜索“cgi.rfc2616_headers”，找到：;cgi.rfc2616_headers = 0 先去前面的分号再改为 cgi.rfc2616_headers = 1
- 搜索“extension=”，找到：;extension=php_mysqli和;extension=pdo_mysql 去掉前面的分号
- 其他的配置请按照自己的需求更改。


> 配置文件里的内容只要去掉最前面的`;`（分号）即为启用相应功能，最后的PHP扩展请根据自己的需求自行启用

5.打开命令提示符`cmd`，切换到PHP目录下，输入命令`php-cgi.exe -b 127.0.0.1:9000-c D:\wnmp\php\php.ini`

6.不要关闭刚刚的cmd窗口，然后重新运行Nginx

7.在www目录下新建一个hello.php文件，内容为`<?php echo "welcome to php";?>`

8.打开浏览器，输入`127.0.0.1/hello.php`，若输出`welcome to php`则说明PHP配置成功

### 优化

PHP服务启动，就必须一直开着启动PHP服务命令的那个cmd窗口，而且，每次开机都要启动Nginx和PHP也会有些繁琐，所以下文介绍如何隐藏cmd窗口，一键启动和关闭Nginx+PHP

下载`RunHiddenConsole`，[蓝奏云](https://rumosky.lanzouy.com/i7lfeohdvpa "蓝奏云")

解压后放置Nginx目录下，文件夹里有两个.bat脚本，内容如下：

start.bat 文件

```bash
@echo off
REM Windows 下无效
REM set PHP_FCGI_CHILDREN=5

REM 每个进程处理的最大请求数，或设置为 Windows 环境变量
set PHP_FCGI_MAX_REQUESTS=1000

echo Starting PHP FastCGI...
RunHiddenConsole D:/wnmp/php/php-cgi.exe -b 127.0.0.1:9000 -c D:/wnmp/php/php.ini

echo Starting nginx...
RunHiddenConsole D:/wnmp/nginx/nginx.exe -p D:/wnmp/nginx
```

stop.bat 文件

```bash
@echo off
echo Stopping nginx...  
taskkill /F /IM nginx.exe > nul
echo Stopping PHP FastCGI...
taskkill /F /IM php-cgi.exe > nul
exit
```

> 若环境配置与本文目录一致，无须修改脚本内容，若不一致，请修改Nginx与PHP路径即可