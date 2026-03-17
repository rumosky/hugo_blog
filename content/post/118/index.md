---
title: "CentOS7下Docker安装Gitea配合宝塔面板实现反代"
slug: "118"
date: 2022-11-29T23:09:00+08:00
lastmod: 2022-11-29T23:09:00+08:00
categories: ["技术"]
tags: ["linux", "gitea", "git", "宝塔面板"]
draft: false
---

## 说明

前几天用二进制安装了Gitea，过程有些繁琐，而且配置SSH也比较麻烦，现在用Docker安装一下，特此记录。

### 步骤

本文使用的环境如下

```txt
Docker version 20.10.17
CentOS 7.9 x64
```

若要使用二进制安装，请参考：[bspost cid="115"]

#### 安装

首先，找一个位置，新建一个目录，例如`/home/git/Gitea/`，在此目录下新建`docker-compose.yml`，内容如下：

```yml
version: "3"

networks:
  gitea:
    external: false

services:
  gitea-app:
    image: gitea/gitea:latest
    ulimits:
      nproc: 65535
      nofile:
        soft: 102400
        hard: 102400
    environment:
      - USER_UID=1000
      - USER_GID=1000
      - GITEA__database__DB_TYPE=mysql
      - GITEA__database__HOST=mysql-server:3306
      - GITEA__database__NAME=git
      - GITEA__database__USER=git
      - GITEA__database__PASSWD=git
    restart: always
    networks:
      - gitea
    volumes:
      - ./gitea-data:/data
      - /etc/timezone:/etc/timezone:ro
      - /etc/localtime:/etc/localtime:ro
    ports:
      - "3000:3000"
      - "22:22"
    depends_on:
      - mysql-server

  mysql-server:
    image: mysql:8
    restart: always
    cap_add:
      - SYS_NICE  # CAP_SYS_NICE, to prevent mbind error
    environment:
      - MYSQL_ALLOW_EMPTY_PASSWORD=yes
      - MYSQL_ROOT_PASSWORD=
      - MYSQL_USER=git
      - MYSQL_PASSWORD=git
      - MYSQL_DATABASE=git
    networks:
      - gitea
    volumes:
      - ./mysql-data:/var/lib/mysql
```

上述文件中，`gitea/gitea:latest`使用的是最新的发行版本，也可以这样`gitea/gitea:1.17.3`，使用具体版本，目前稳定版为1.17.3；`ports`后面的两个端口，其中`"3000:3000"`是http映射端口，后面的不要修改，前面的可以修改为8080，80等端口，可自定义；`"22:22"`是ssh端口，官方默认是`"2222:22"`，具体原因请至文末查看相关内容。

开始安装，执行

```bash
docker-compose up -d
```

此时，访问`ip:3000`就可以看到Gitea了，若在上面修改了端口号，就以修改后的端口为准

使用域名访问，请添加Nginx反代，这里用宝塔面板作为例子，新建一个站点，填写好域名，数据库不勾选，php选择纯静态，提交

修改配置文件，在`server`中添加：

```bash
location / {
        proxy_pass http://127.0.0.1:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
```

还需要修改css js和图片处，完整配置文件如下（已开启ssl）：

```bash
server
{
    listen 80;
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    listen [::]:80;
    server_name example.com;
    index index.php index.html index.htm default.php default.htm default.html;
    root /www/wwwroot/example.com;
    
    location / {
        proxy_pass http://127.0.0.1:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    #SSL-START SSL相关配置，请勿删除或修改下一行带注释的404规则
    #error_page 404/404.html;
    #HTTP_TO_HTTPS_START
    if ($server_port !~ 443){
        rewrite ^(/.*)$ https://$host$1 permanent;
    }
    #HTTP_TO_HTTPS_END
    ssl_certificate    /www/server/panel/vhost/cert/example.com/fullchain.pem;
    ssl_certificate_key    /www/server/panel/vhost/cert/example.com/privkey.pem;
    ssl_protocols TLSv1.1 TLSv1.2 TLSv1.3;
    ssl_ciphers EECDH+CHACHA20:EECDH+CHACHA20-draft:EECDH+AES128:RSA+AES128:EECDH+AES256:RSA+AES256:EECDH+3DES:RSA+3DES:!MD5;
    ssl_prefer_server_ciphers on;
    ssl_session_cache shared:SSL:10m;
    ssl_session_timeout 10m;
    add_header Strict-Transport-Security "max-age=31536000";
    error_page 497  https://$host$request_uri;
    #SSL-END

    #ERROR-PAGE-START  错误页配置，可以注释、删除或修改
    #error_page 404 /404.html;
    #error_page 502 /502.html;
    #ERROR-PAGE-END

    #PHP-INFO-START  PHP引用配置，可以注释或修改
    include enable-php-00.conf;
    #PHP-INFO-END

    #REWRITE-START URL重写规则引用,修改后将导致面板设置的伪静态规则失效
    include /www/server/panel/vhost/rewrite/git.rumosky.com.conf;
    #REWRITE-END

    #禁止访问的文件或目录
    location ~ ^/(\.user.ini|\.htaccess|\.git|\.env|\.svn|\.project|LICENSE|README.md)
    {
        return 404;
    }

    #一键申请SSL证书验证目录相关设置
    location ~ \.well-known{
        allow all;
    }

    #禁止在证书验证目录放入敏感文件
    if ( $uri ~ "^/\.well-known/.*\.(php|jsp|py|js|css|lua|ts|go|zip|tar\.gz|rar|7z|sql|bak)$" ) {
        return 403;
    }

    location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
    {
        proxy_pass http://127.0.0.1:3000;
        expires      30d;
        error_log /dev/null;
        access_log /dev/null;
    }

    location ~ .*\.(js|css)?$
    {
        proxy_pass http://127.0.0.1:3000;
        expires      12h;
        error_log /dev/null;
        access_log /dev/null;
    }
    access_log  /www/wwwlogs/example.com.log;
    error_log  /www/wwwlogs/example.com.error.log;
}
```

至此，就可以使用域名访问了

### 补充

相比于二进制安装，述安装方式更加简单高效，升级迁移也方便。

#### 升级/迁移

首先，备份gitea数据`/home/git/Gitea/gitea-data/`和mysql数据`/home/git/Gitea/mysql-data`

执行

```bash
docker-compose pull
```

运行

```bash
docker-compose up -d
```

也可以修改`docker-compose.yml`文件中`gitea/gitea:latest`部分，然后再执行运行命令即可

#### 其他命令

查看Docker-compose管理的容器

```bash
docker-compose ps
```

查看Docker-compose日志

```bash
docker-compose logs
```

关闭并删除容器

```bash
docker-compose down
```

开启、重启、停止正在运行的容器

```bash
docker-compose start
docker-compose restart
docker-compose stop
```

## 结语

Docker方式比二进制方式遇到的BUG少很多，但相对来说，自定义SSH端口配置起来也比较麻烦，在这里就不多介绍SSH配置了，问题较多，单独成文，请参考：[bspost cid="119"]