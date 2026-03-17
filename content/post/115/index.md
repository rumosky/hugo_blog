---
title: "CentOS7下二进制安装Gitea配合宝塔面板实现反代"
slug: "115"
date: 2022-11-29T10:37:00+08:00
lastmod: 2022-11-29T10:37:00+08:00
categories: ["技术"]
tags: ["linux", "gitea", "git", "宝塔面板"]
draft: false
---

## 前言

最近因为个人原因，在频繁的写和更改代码，代码这种重要的东西，自然是需要多多备份的，但是，由于众所周知的原因，GitHub的上传和拉取速度实在感人，而无论是GitHub还是Gitee，都不能无限制私人仓库，所以准备自建Git，考虑过GitLab，专业好用，但却是太大了，而且对配置要求比较高，个人使用没必要；国产的Code Fever社区用户太少，内存至少1800M，而且，侧重于团队协作，需要php环境，遂放弃。

### 步骤

本文采用二进制文件安装，Docker安装请参考：[bspost cid="118"]

#### 环境

本文使用环境如下

```txt
系统：CentOS 7.9 x64
数据库：Mysql 5.7.39
Web服务：Nginx 1.20.2
Git服务：Git 2.38.1
Linux管理面板：宝塔7.9.6
```

#### 安装

先确保系统Git是2.x版本，默认的1.8.x无法安装Gitea，参考：[bspost cid="116"]

Gitea默认不允许使用root账户，先新建用户

```bash
adduser gitea
```

设置密码

```bash
passwd gitea
```

新用户需要配置sudo权限，用root用户执行

```bash
vi /etc/sudoers
```

找到第99行，`Allow root to run any commands anywhere`下面添加`gitea   ALL=(ALL)   ALL`，完整文件内容如下：

```bash
## Sudoers allows particular users to run various commands as
## the root user, without needing the root password.
##
## Examples are provided at the bottom of the file for collections
## of related commands, which can then be delegated out to particular
## users or groups.
## 
## This file must be edited with the 'visudo' command.

## Host Aliases
## Groups of machines. You may prefer to use hostnames (perhaps using 
## wildcards for entire domains) or IP addresses instead.
# Host_Alias     FILESERVERS = fs1, fs2
# Host_Alias     MAILSERVERS = smtp, smtp2

## User Aliases
## These aren't often necessary, as you can use regular groups
## (ie, from files, LDAP, NIS, etc) in this file - just use %groupname 
## rather than USERALIAS
# User_Alias ADMINS = jsmith, mikem


## Command Aliases
## These are groups of related commands...

## Networking
# Cmnd_Alias NETWORKING = /sbin/route, /sbin/ifconfig, /bin/ping, /sbin/dhclient, /usr/bin/net, /sbin/iptables, /usr/bin/rfcomm, /usr/bin/wvdial, /sbin/iwconfig, /sbin/mii-tool

## Installation and management of software
# Cmnd_Alias SOFTWARE = /bin/rpm, /usr/bin/up2date, /usr/bin/yum

## Services
# Cmnd_Alias SERVICES = /sbin/service, /sbin/chkconfig, /usr/bin/systemctl start, /usr/bin/systemctl stop, /usr/bin/systemctl reload, /usr/bin/systemctl restart, /usr/bin/systemctl status, /usr/bin/systemctl enable, /usr/bin/systemctl disable

## Updating the locate database
# Cmnd_Alias LOCATE = /usr/bin/updatedb

## Storage
# Cmnd_Alias STORAGE = /sbin/fdisk, /sbin/sfdisk, /sbin/parted, /sbin/partprobe, /bin/mount, /bin/umount

## Delegating permissions
# Cmnd_Alias DELEGATING = /usr/sbin/visudo, /bin/chown, /bin/chmod, /bin/chgrp 

## Processes
# Cmnd_Alias PROCESSES = /bin/nice, /bin/kill, /usr/bin/kill, /usr/bin/killall

## Drivers
# Cmnd_Alias DRIVERS = /sbin/modprobe

# Defaults specification

#
# Refuse to run if unable to disable echo on the tty.
#
Defaults   !visiblepw

#
# Preserving HOME has security implications since many programs
# use it when searching for configuration files. Note that HOME
# is already set when the the env_reset option is enabled, so
# this option is only effective for configurations where either
# env_reset is disabled or HOME is present in the env_keep list.
#
Defaults    always_set_home
Defaults    match_group_by_gid

# Prior to version 1.8.15, groups listed in sudoers that were not
# found in the system group database were passed to the group
# plugin, if any. Starting with 1.8.15, only groups of the form
# %:group are resolved via the group plugin by default.
# We enable always_query_group_plugin to restore old behavior.
# Disable this option for new behavior.
Defaults    always_query_group_plugin

Defaults    env_reset
Defaults    env_keep =  "COLORS DISPLAY HOSTNAME HISTSIZE KDEDIR LS_COLORS"
Defaults    env_keep += "MAIL PS1 PS2 QTDIR USERNAME LANG LC_ADDRESS LC_CTYPE"
Defaults    env_keep += "LC_COLLATE LC_IDENTIFICATION LC_MEASUREMENT LC_MESSAGES"
Defaults    env_keep += "LC_MONETARY LC_NAME LC_NUMERIC LC_PAPER LC_TELEPHONE"
Defaults    env_keep += "LC_TIME LC_ALL LANGUAGE LINGUAS _XKB_CHARSET XAUTHORITY"

#
# Adding HOME to env_keep may enable a user to run unrestricted
# commands via sudo.
#
# Defaults   env_keep += "HOME"

Defaults    secure_path = /sbin:/bin:/usr/sbin:/usr/bin

## Next comes the main part: which users can run what software on 
## which machines (the sudoers file can be shared between multiple
## systems).
## Syntax:
##
## 	user	MACHINE=COMMANDS
##
## The COMMANDS section may have other options added to it.
##
## Allow root to run any commands anywhere 
root	ALL=(ALL)	ALL
gitea	ALL=(ALL)	ALL

## Allows members of the 'sys' group to run networking, software, 
## service management apps and more.
# %sys ALL = NETWORKING, SOFTWARE, SERVICES, STORAGE, DELEGATING, PROCESSES, LOCATE, DRIVERS

## Allows people in group wheel to run all commands
%wheel	ALL=(ALL)	ALL

## Same thing without a password
# %wheel	ALL=(ALL)	NOPASSWD: ALL

## Allows members of the users group to mount and unmount the 
## cdrom as root
# %users  ALL=/sbin/mount /mnt/cdrom, /sbin/umount /mnt/cdrom

## Allows members of the users group to shutdown this system
# %users  localhost=/sbin/shutdown -h now

## Read drop-in files from /etc/sudoers.d (the # here does not mean a comment)
#includedir /etc/sudoers.d
lighthouse ALL=(ALL) NOPASSWD: ALL

```

> 在编辑`sudoers`文件时，会提示`W10: Warning: Changing a readonly file`，此时有两种解决方式：1，编辑之后强制保存退出 `:wq!` 2，用`chmod u+w /etc/sudoers`增加权限，编辑完之后记得修改回来，`chmod u-w /etc/sudoers`

此时，用户目录为`/home/gitea/`，此目录为Gitea主目录，执行`su gitea`切换至`gitea`用户

从[下载页面](https://dl.gitea.io/gitea)选择对应平台，拷贝下载URL，执行：

```bash
wget -O gitea https://dl.gitea.io/gitea/1.17.3/gitea-1.17.3-linux-amd64
```

添加可执行权限：

```bash
chmod +x gitea
```

测试运行：

```bash
./gitea web
```

此时访问IP:3000即可访问Gitea，确保3000默认端口开放，若可正常访问，先初始化系统设置，然后`Ctrl C`结束运行，将这个程序作为服务运行，要不然此窗口一关闭程序就停止了。

> 初始化系统时需要填写数据库信息，可在宝塔面板中新建数据库，字符集`utf8mb4`

为了程序可以方便的后台运行，需要将程序注册为服务来运行，执行：

```bash
sudo vim /etc/systemd/system/gitea.service
```

官方实例服务配置文件如下：

```bash
[Unit]
Description=Gitea (Git with a cup of tea)
After=syslog.target
After=network.target
###
# Don't forget to add the database service dependencies
###
#
#Wants=mysql.service
#After=mysql.service
#
#Wants=mariadb.service
#After=mariadb.service
#
#Wants=postgresql.service
#After=postgresql.service
#
#Wants=memcached.service
#After=memcached.service
#
#Wants=redis.service
#After=redis.service
#
###
# If using socket activation for main http/s
###
#
#After=gitea.main.socket
#Requires=gitea.main.socket
#
###
# (You can also provide gitea an http fallback and/or ssh socket too)
#
# An example of /etc/systemd/system/gitea.main.socket
###
##
## [Unit]
## Description=Gitea Web Socket
## PartOf=gitea.service
##
## [Socket]
## Service=gitea.service
## ListenStream=<some_port>
## NoDelay=true
##
## [Install]
## WantedBy=sockets.target
##
###

[Service]
# Uncomment the next line if you have repos with lots of files and get a HTTP 500 error because of that
# LimitNOFILE=524288:524288
RestartSec=2s
Type=simple
User=git
Group=git
WorkingDirectory=/var/lib/gitea/
# If using Unix socket: tells systemd to create the /run/gitea folder, which will contain the gitea.sock file
# (manually creating /run/gitea doesn't work, because it would not persist across reboots)
#RuntimeDirectory=gitea
ExecStart=/usr/local/bin/gitea web --config /etc/gitea/app.ini
Restart=always
Environment=USER=git HOME=/home/git GITEA_WORK_DIR=/var/lib/gitea
# If you install Git to directory prefix other than default PATH (which happens
# for example if you install other versions of Git side-to-side with
# distribution version), uncomment below line and add that prefix to PATH
# Don't forget to place git-lfs binary on the PATH below if you want to enable
# Git LFS support
#Environment=PATH=/path/to/git/bin:/bin:/sbin:/usr/bin:/usr/sbin
# If you want to bind Gitea to a port below 1024, uncomment
# the two values below, or use socket activation to pass Gitea its ports as above
###
#CapabilityBoundingSet=CAP_NET_BIND_SERVICE
#AmbientCapabilities=CAP_NET_BIND_SERVICE
###
# In some cases, when using CapabilityBoundingSet and AmbientCapabilities option, you may want to
# set the following value to false to allow capabilities to be applied on gitea process. The following
# value if set to true sandboxes gitea service and prevent any processes from running with privileges
# in the host user namespace.
###
#PrivateUsers=false
###

[Install]
WantedBy=multi-user.target
```

修改`Service`中用户和对应目录的内容，完整配置内容如下：

```bash
[Unit]
Description=Codes (GitHub of rumosky)
After=syslog.target
After=network.target
###
# Don't forget to add the database service dependencies
###
#
#Wants=mysql.service
#After=mysql.service
#
#Wants=mariadb.service
#After=mariadb.service
#
#Wants=postgresql.service
#After=postgresql.service
#
#Wants=memcached.service
#After=memcached.service
#
#Wants=redis.service
#After=redis.service
#
###
# If using socket activation for main http/s
###
#
#After=gitea.main.socket
#Requires=gitea.main.socket
#
###
# (You can also provide gitea an http fallback and/or ssh socket too)
#
# An example of /etc/systemd/system/gitea.main.socket
###
##
## [Unit]
## Description=Gitea Web Socket
## PartOf=gitea.service
##
## [Socket]
## Service=gitea.service
## ListenStream=<some_port>
## NoDelay=true
##
## [Install]
## WantedBy=sockets.target
##
###

[Service]
# Uncomment the next line if you have repos with lots of files and get a HTTP 500 error because of that
# LimitNOFILE=524288:524288
RestartSec=2s
Type=simple
User=gitea
Group=gitea
WorkingDirectory=/home/gitea/
# If using Unix socket: tells systemd to create the /run/gitea folder, which will contain the gitea.sock file
# (manually creating /run/gitea doesn't work, because it would not persist across reboots)
#RuntimeDirectory=gitea
ExecStart=/home/gitea/gitea web --config /home/gitea/custom/conf/app.ini
Restart=always
Environment=USER=gitea HOME=/home/gitea/ GITEA_WORK_DIR=/home/gitea/
# If you install Git to directory prefix other than default PATH (which happens
# for example if you install other versions of Git side-to-side with
# distribution version), uncomment below line and add that prefix to PATH
# Don't forget to place git-lfs binary on the PATH below if you want to enable
# Git LFS support
#Environment=PATH=/path/to/git/bin:/bin:/sbin:/usr/bin:/usr/sbin
# If you want to bind Gitea to a port below 1024, uncomment
# the two values below, or use socket activation to pass Gitea its ports as above
###
#CapabilityBoundingSet=CAP_NET_BIND_SERVICE
#AmbientCapabilities=CAP_NET_BIND_SERVICE
###
# In some cases, when using CapabilityBoundingSet and AmbientCapabilities option, you may want to
# set the following value to false to allow capabilities to be applied on gitea process. The following
# value if set to true sandboxes gitea service and prevent any processes from running with privileges
# in the host user namespace.
###
#PrivateUsers=false
###

[Install]
WantedBy=multi-user.target

```

> 注意：`ExecStart`中`config`参数的`app.ini`配置文件，是测试时初始化系统生成的，路径是默认位置，若测试时未初始化系统，则复制下述配置文件，修改对应信息即可（提示，邮件配置host地址之后需要加上端口号，18+版本以上则不需要）

```bash
APP_NAME = Codes: GitHub of rumosky
RUN_USER = gitea
RUN_MODE = prod

[database]
DB_TYPE  = mysql
HOST     = 127.0.0.1:3306
NAME     = xxx
USER     = xxx
PASSWD   = xxx
SCHEMA   = 
SSL_MODE = disable
CHARSET  = utf8mb4
PATH     = /home/gitea/data/gitea.db
LOG_SQL  = false

[repository]
ROOT = /home/gitea/data/gitea-repositories

[server]
SSH_DOMAIN       = xxx.com
DOMAIN           = xxx.com
HTTP_PORT        = 3000
ROOT_URL         = https://xxx.com/
DISABLE_SSH      = false
SSH_PORT         = 22
LFS_START_SERVER = true
LFS_JWT_SECRET   = ******
OFFLINE_MODE     = false

[lfs]
PATH = /home/gitea/data/lfs

[mailer]
ENABLED = true
HOST    = ***.com:port
FROM    = ***@example.com
USER    = ***@example.com
PASSWD  = xxx

[service]
REGISTER_EMAIL_CONFIRM            = true
ENABLE_NOTIFY_MAIL                = true
DISABLE_REGISTRATION              = true
ALLOW_ONLY_EXTERNAL_REGISTRATION  = false
ENABLE_CAPTCHA                    = false
REQUIRE_SIGNIN_VIEW               = false
DEFAULT_KEEP_EMAIL_PRIVATE        = false
DEFAULT_ALLOW_CREATE_ORGANIZATION = false
DEFAULT_ENABLE_TIMETRACKING       = true
NO_REPLY_ADDRESS                  = noreply.example.com

[picture]
DISABLE_GRAVATAR        = true
ENABLE_FEDERATED_AVATAR = false

[openid]
ENABLE_OPENID_SIGNIN = false
ENABLE_OPENID_SIGNUP = false

[session]
PROVIDER = file

[log]
MODE      = console
LEVEL     = info
ROOT_PATH = /home/gitea/log
ROUTER    = console

[repository.pull-request]
DEFAULT_MERGE_STYLE = merge

[repository.signing]
DEFAULT_TRUST_MODEL = committer

[security]
INSTALL_LOCK       = true
INTERNAL_TOKEN     = *****
PASSWORD_HASH_ALGO = pbkdf2

```

保存成功后，打开宝塔面板，新建一个站点，由于在前面测试时已经新建了数据库，此处就不再新建数据库，php选择纯静态，新建成功后，修改配置文件，在`server`中添加：

```bash
location / {
        proxy_pass http://127.0.0.1:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
```

注意，宝塔还需要修改css js和图片处，完整的配置文件内容如下（已配置ssl）：

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

此时已经配置好了Nginx反代，等启动服务之后就可以使用`example.com`访问Gitea

接下来继续配置服务，官方提示`接着拷贝示例代码 gitea.service 并取消对任何需要运行在主机上的服务部分的注释，譬如 MySQL。`这一项，对于宝塔面板来说不需要执行，亲测修改mysql部分内容后无法启动服务

激活服务

```bash
sudo systemctl enable gitea
```

运行服务

```bash
sudo systemctl start gitea
```

这时就可以通过域名访问了

#### 补充

若修改了服务的配置文件，则需要先停止服务

```bash
sudo systemctl stop gitea.service
```

重新加载服务之后再运行服务，重新加载服务命令：

```bash
sudo systemctl daemon-reload
```

查看服务状态

```bash
sudo systemctl status gitea.service
```

服务在活动状态，但报错，结果如下：

```bash
● gitea.service - Codes (GitHub of rumosky)
   Loaded: loaded (/etc/systemd/system/gitea.service; enabled; vendor preset: disabled)
   Active: activating (auto-restart) (Result: exit-code) since Tue 2022-11-29 09:28:36 CST; 149ms ago
  Process: 21386 ExecStart=/home/gitea/gitea web (code=exited, status=1/FAILURE)
 Main PID: 21386 (code=exited, status=1/FAILURE)
```

服务正常运行状态如下：

```bash
● gitea.service - Codes (GitHub of rumosky)
   Loaded: loaded (/etc/systemd/system/gitea.service; enabled; vendor preset: disabled)
   Active: active (running) since Tue 2022-11-29 10:16:56 CST; 2s ago
 Main PID: 10117 (gitea)
    Tasks: 8
   Memory: 126.9M
   CGroup: /system.slice/gitea.service
           └─10117 /home/gitea/gitea web --config /home/gitea/custom/conf/app.ini
```

注意，报错时状态会显示红色，正常运行则为绿色

#### supervisor

官网给了supervisor守护进程的配置方法，这里附上修改后的配置文件，供参考：

```bash
[program:gitea]
directory=/home/gitea/
command=/home/gitea/gitea web
autostart=true
autorestart=true
startsecs=10
stdout_logfile=/home/gitea/log/supervisor/gitea.out.log
stdout_logfile_maxbytes=1MB
stdout_logfile_backups=10
stdout_capture_maxbytes=1MB
stderr_logfile=/home/gitea/log/supervisor/gitea.err.log
stderr_logfile_maxbytes=1MB
stderr_logfile_backups=10
stderr_capture_maxbytes=1MB
user = gitea
environment = HOME="/home/gitea", USER="gitea"
```

安装过程就不再赘述，最后激活 supervisor 并将它作为系统自启动服务：

```bash
sudo systemctl enable supervisor
```

```bash
sudo systemctl start supervisor
```

### 日志配置

默认的日志配置之后log文件夹下是空的，需要修改`app.ini`，内容如下：

```bash
[log]
LEVEL=debug
MODE=console,file
ROUTER=console,file
XORM=console,file
ENABLE_XORM_LOG=true
FILE_NAME=gitea.log
[log.file.router]
FILE_NAME=router.log
[log.file.xorm]
FILE_NAME=xorm.log
```

## 结语

折腾了两天时间，最后弄好了，查了很多资料，官方的文档有很多步骤都是略过的，可能默认都懂。

最难的就是使用自定义SSH端口，默认22端口按照上述过程无问题，修改后请参考：[bspost cid="119"]