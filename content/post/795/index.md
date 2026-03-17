---
title: "docker正确配置腾讯云镜像地址"
slug: "795"
date: 2024-06-06T10:05:00+08:00
lastmod: 2024-06-06T10:39:30+08:00
categories: ["技术"]
tags: ["docker", "国内镜像", "腾讯云"]
draft: false
---

## 说明

腾讯软件源官网给的配置教程有误，配置完成之后还是显示无法连接，切换了一下方式才成功，记录一下。

### 官网文档

附上官网文档地址：[腾讯云修改docker镜像文档](https://cloud.tencent.com/document/product/213/8623#.E4.BD.BF.E7.94.A8.E8.85.BE.E8.AE.AF.E4.BA.91.E9.95.9C.E5.83.8F.E6.BA.90.E5.8A.A0.E9.80.9F-docker)

附上腾讯云软件源官网：[腾讯软件源](https://mirrors.cloud.tencent.com/)

下面是详细步骤，Ubuntu系统：

```bash
# 打开配置文件
vim /etc/default/docker

# 添加以下内容
DOCKER_OPTS="--registry-mirror=https://mirror.ccs.tencentyun.com"
```

CentOS7系统：

```bash
# 打开配置文件
vim /etc/docker/daemon.json

# 添加以下内容
{
   "registry-mirrors": [
   "https://mirror.ccs.tencentyun.com"
  ]
}
```

修改完配置文件，重启一下docker

```bash
systemctl daemon-reload
systemctl restart docker
```

### 验证

执行

```bash
docker info
```

输出的内容最后三行有这个就表示配置成功：

```bash
 Registry Mirrors:
  https://mirror.ccs.tencentyun.com/
 Live Restore Enabled: false
```

## 补充

我的系统是Ubuntu22.04，但是按照Ubuntu系统那样配置没生效，而且，那个配置文件默认是有内容的，只需要把注释符号删掉修改一下就可以了，即第14行，原始内容如下：

```bash
# Docker SysVinit configuration file

#
# THIS FILE DOES NOT APPLY TO SYSTEMD
#
#   Please see the documentation for "systemd drop-ins":
#   https://docs.docker.com/engine/admin/systemd/
#

# Customize location of Docker binary (especially for development testing).
#DOCKERD="/usr/local/bin/dockerd"

# Use DOCKER_OPTS to modify the daemon startup options.
#DOCKER_OPTS="--dns 8.8.8.8 --dns 8.8.4.4"

# If you need Docker to use an HTTP proxy, it can also be specified here.
#export http_proxy="http://127.0.0.1:3128/"

# This is also a handy place to tweak where Docker's temporary files go.
#export DOCKER_TMPDIR="/mnt/bigdrive/docker-tmp"

```

但我换成CentOS那样的配置文件就生效了，所以我查看了一下上面文件里的docker官方文档，文档显示，这个文件是配置docker守护进程的，需要添加一个守护进程，而守护进程使用的配置文件就是这个`/etc/docker/daemon.json`，所以腾讯云的文档不太严谨，可能是没有更新，官网写的docker23.0以上都是这样。以后还是建议查看docker官网文档。配置守护进程验证如下：

```bash
sudo systemctl show --property=Environment docker

Environment=HTTP_PROXY=http://proxy.example.com:3128 HTTPS_PROXY=https://proxy.example.com:3129 NO_PROXY=localhost,127.0.0.1,docker-registry.example.com,.corp
```

相应的配置文件如下：

```conf
[Service]
Environment="HTTP_PROXY=http://proxy.example.com:3128"
Environment="HTTPS_PROXY=https://proxy.example.com:3129"
Environment="NO_PROXY=localhost,127.0.0.1,docker-registry.example.com,.corp"
```

文件路径为：`/etc/systemd/system/docker.service.d/http-proxy.conf`