---
title: "mindoc接口文档在线管理系统（使用宝塔面板安装配置教程）"
slug: "12"
date: 2019-01-03T10:31:00+08:00
lastmod: 2019-01-03T10:31:00+08:00
categories: ["技术"]
tags: ["宝塔面板", "mindoc"]
draft: false
---

## mindoc简介

MinDoc 是一款针对IT团队开发的简单好用的文档管理系统。

地址：[mindoc](https://github.com/mindoc-org/mindoc)

### 安装教程

本文以centOS7为例，根据自己安装的经验以及官网文档修改而来，不懂的地方请自行阅读官网文档

#### 安装环境

远程连接到你的服务器，先执行

```bash
yum update -y # 更新系统
```

##### 安装Calibre

在安装之前，依次执行：

```bash
yum install python2.6 -y
yum install libstdc++.so.6.0.17 -y
yum install GLIBC 2.17 -y
yum install libXcomposite -y
yum install libGL
```

> 如果最后提示`Nothing to do`说明你的系统已经安装了该软件，请忽略，执行下一步

确保依赖关系安装上之后，请执行

```bash
sudo -v && wget -nv -O- https://download.calibre-ebook.com/linux-installer.py | sudo python -c "import sys; main=lambda:sys.stderr.write('Download failed\n'); exec(sys.stdin.read()); main()"
```

如果安装成功，执行`ebook-convert --version`之后会看到`3.x`的版本号

#### 下载程序

请在本文开头处下载，截止发稿，mindoc最新的正式版本为1.0.2，最新的测试版本为2.0-beta2

#### 宝塔配置

- 首先，确保你的服务器上已经安装了宝塔面板，不会安装的请自行百度。
- 在www/wwwroot目录下新建一个文件夹，然后将下载好的压缩包上传到那个文件夹，然后解压到该文件夹根目录
- 新建一个数据库，数据库字符集请选择`utf8mb4`，然后记住数据库名，用户名，密码
- 修改配置文件，在刚刚建立的文件夹下找到`mindoc`的配置文件，位置是`conf/app.conf`文件，将刚刚得到的数据库信息填写进去


```bash
####################MySQL 数据库配置###########################
#支持MySQL和sqlite3两种数据库，如果是sqlite3 则 db_database 标识数据库的物理目录
db_adapter="${MINDOC_DB_ADAPTER||mysql}"
db_host="${MINDOC_DB_HOST||127.0.0.1}"
db_port="${MINDOC_DB_PORT||3306}"
db_database="${MINDOC_DB_DATABASE||mindoc_db}"
db_username="${MINDOC_DB_USERNAME||mindoc}"
db_password="${MINDOC_DB_PASSWORD||123456}"
```

- 修改环境变量，在宝塔文件功能里面，浏览系统文件，找到根目录`/etc`下的`profile`文件，在文件最后一行添加：


```bash
export PATH=$PATH:/www/wwwroot/你建立的目录/lib/time/zoneinfo.zip
```

然后保存文件，执行`source /etc/profile`，这样就可以让环境变量立即生效

- 进入你放mindoc安装程序的目录，在终端页面执行`./mindoc_linux_amd64 install`来初始化数据库

- 执行下面两条命令


```bash
#修改可执行权限
chmod +x mindoc_linux_amd64

#启动程序
./mindoc_linux_amd64
```

- 在宝塔面板网站选项卡上新建一个网站，网站域名自己写，目录请选择刚刚存放mindoc的那个目录，数据库和ftp都不要选。
- 配置Nginx代理，点击刚刚创建好的网站的设置按钮，在配置文件里面写入如下代码


```nginx
server {
    listen       80;

    #此处应该配置你的域名：
    server_name  doc.124.221.171.144;

    #SSL-START SSL相关配置，请勿删除或修改下一行带注释的404规则
    #error_page 404/404.html;
    #SSL-END

    charset utf-8;

    #此处配置你的访问日志，请手动创建该目录：
    access_log  /www/wwwlogs/doc_rumosky_com.log;
    location ~ .*\.(ttf|woff2|eot|otf|map|swf|svg|gif|jpg|jpeg|bmp|ico|txt|js|css)$ 
    {
        root "/www/wwwroot/你建立的目录";
        expires 30m;
    }

    location / {
        try_files /_not_exists_ @backend;
    }

    # 这里为具体的服务代理配置
    location @backend {
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_set_header Host            $http_host;
        proxy_set_header   X-Forwarded-Proto $scheme;

        #此处配置 MinDoc 程序的地址和端口号
        proxy_pass http://127.0.0.1:8181;
    }
}
```

然后点击保存，现在通过你刚刚设置的域名就可以访问mindoc文档系统了。

 - 配置文件里面的#SSL-START和#SSL-END是为了开启ssl证书添加的，如果不添加此代码，在宝塔面板一键开启ssl证书会失败
 - 在执行`./mindoc_linux_amd64`命令启动程序之后，不能关闭终端，否则程序会关闭，就无法访问了。解决办法就是让它后台运行，偷懒的做法就是执行`./mindoc_linux_amd64 &amp;`就后台运行。缺点就是服务器重启，mindoc就断开了，需要手动再次运行。
 - 推荐使用官方的解决办法，安装mindoc服务，让mindoc以服务后台运行，优点是即便重启服务器也会保持运行。


#### 安装mindoc服务

```bash
# 安装服务
./mindoc_linux_amd64 service install
# 卸载服务
./mindoc_linux_amd64 service remove
```

#### 设置mindoc服务开机启动

 - 在根目录`/etc/rc.d`里面找到`rc.local`文件，在最后一行添加`service mindocd start`，然后保存文件即可。
 - 查看服务状态的命令`service mindocd status`


为了更好的用户体验，mindoc进阶配置参考：[bspost cid="13"]

若想要去除默认的标签页版权，请参考：[bspost cid="24"]