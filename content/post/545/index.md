---
title: "Alist自定义编译打包docker镜像"
slug: "545"
date: 2023-04-27T15:51:00+08:00
lastmod: 2024-05-05T00:54:30+08:00
categories: ["技术"]
tags: ["linux", "nodejs", "docker", "alist"]
draft: false
---

## 说明

最近帮别人搭建`alist`，需要定制化一些内容，基于源码编译安装，打包成`docker`镜像，并且发布到`dockehub`上。

### 步骤

写在最前面，官方的文档十分详细，还有视频教程，请仔细阅读，本文只是补充。

情况说明：本文是在使用`GitHub`的`codespace`时报错`317`内存不够，没办法编译前端，所以无法进行下一步，只能自己在Linux进行编译安装

本文内容是在`Windows`电脑上，用WSL安装的`Ubuntu-22.04`，环境配置如下：

```txt
OS: Ubuntu-22.04
nodejs: 18.16
git: 2.34.1
golang: 1.19
pnpm: 8.3.1
docker: 20.10.24
```

上述环境请自行安装配置，本文不在赘述。

1.克隆前后端代码：

```bash
# 后端
git clone https://github.com/alist-org/alist.git

# 前端
git clone --recurse-submodules https://github.com/alist-org/alist-web.git
```

2.进入前端项目文件夹，下载中文语言包并应用，执行：

```bash
cd alist-web 
wget https://crowdin.com/backend/download/project/alist/zh-CN.zip 
unzip zh-CN.zip 
node ./scripts/i18n.mjs
rm zh-CN.zip
```

3.自行修改前端自定义的部分，保存，执行：

```bash
pnpm install && pnpm build
```

4.将打包的`dist`目录复制到后端项目`public`文件下，命令行执行参考：

```bash
cp -r /前端路径/dist/ /后端路径/public/
```

5.进入后端项目，开始编译，执行：

```bash
appName="alist"
builtAt="$(date +'%F %T %z')"
goVersion=$(go version | sed 's/go version //')
gitAuthor=$(git show -s --format='format:%aN <%ae>' HEAD)
gitCommit=$(git log --pretty=format:"%h" -1)
version=$(git describe --long --tags --dirty --always)
webVersion=$(wget -qO- -t1 -T2 "https://api.github.com/repos/alist-org/alist-web/releases/latest" | grep "tag_name" | head -n 1 | awk -F ":" '{print $2}' | sed 's/\"//g;s/,//g;s/ //g')
ldflags="\
-w -s \
-X 'github.com/alist-org/alist/v3/internal/conf.BuiltAt=$builtAt' \
-X 'github.com/alist-org/alist/v3/internal/conf.GoVersion=$goVersion' \
-X 'github.com/alist-org/alist/v3/internal/conf.GitAuthor=$gitAuthor' \
-X 'github.com/alist-org/alist/v3/internal/conf.GitCommit=$gitCommit' \
-X 'github.com/alist-org/alist/v3/internal/conf.Version=$version' \
-X 'github.com/alist-org/alist/v3/internal/conf.WebVersion=$webVersion' \
"
go build -ldflags="$ldflags" .

```

如果没有报错的话，会生成一个`alist`二进制文件，这个就是编译成功的可执行文件

> 此时最好测试一下编译好的文件是否可以正常运行，执行`./alist admin`查看管理员密码，执行`./alist server`即可访问

6.新建一个`Dockerfile`文件，内容如下：

```bash
FROM ubuntu:22.04
LABEL MAINTAINER="rumosky@163.com"
VOLUME /opt/alist/data/
WORKDIR /opt/alist/
COPY alist ./
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh
ENV PUID=0 PGID=0 UMASK=022
EXPOSE 5244
CMD [ "/entrypoint.sh" ]
```

7.新建一个`entrypoint.sh`文件，内容如下：

```bash
#!/bin/bash

chown -R ${PUID}:${PGID} /opt/alist/

umask ${UMASK}

./alist server --no-prefix
```

> 这里补充一下，后来我发现Dockerfile和entrypoint.sh脚本是可以写在一起的，修改后的Dockerfile如下：

```bash
FROM ubuntu:22.04
LABEL MAINTAINER="rumosky@163.com"
VOLUME /opt/alist/data/
WORKDIR /opt/alist/
COPY ./alist /opt/alist/
RUN chmod +x /opt/alist/alist
ENV PUID=0 PGID=0 UMASK=022
EXPOSE 5244
ENTRYPOINT ["/bin/bash","-c","chown -R ${PUID}:${PGID} /opt/alist/ && umask ${UMASK} && /opt/alist/alist server --no-prefix"]
```

这时，只需要Dockerfile就好了

8.制作`docker`镜像，执行：

```bash
docker build -t rumosky/alist:1.0 .
```

9.在`dockerhub`或其他私有仓库中新建一个仓库，命名为`alist`

10.推送镜像到公共仓库，执行：

```bash
docker push rumosky/alist:1.0
```

至此，就可以在任何地方，从`dockerhub`中`pull`镜像了。

> 推送时如果提示`access deny`，需要登录一下，执行`docker login`，输入用户名和密码即可

本文构建的镜像地址如下，想用的话，随便用

```bash
docker pull rumosky/alist:latest
```

或者使用`docker-compose.yml`，内容如下：

```yml
version: '3.3'
services:
    alist:
        restart: always
        volumes:
            - '/alist/data:/opt/alist/data'
        ports:
            - '5244:5244'
        environment:
            - PUID=0
            - PGID=0
            - UMASK=022
        container_name: alist
        image: 'rumosky/alist:latest'
```

使用`docker-compose up -d`启动，使用`docker-compose down`关闭并删除容器

#### 补充

构建镜像时，最好是用户名/仓库名:tag的格式，这样方便直接推送，但是如果写错了，也可以进行修改，先使用`docker images`查看当前镜像的ID，然后执行`docker tag ID 用户名/仓库名:tag`来重命名。

构建镜像时的FROM，可以为golang等内容，这个自行调整，只要能运行即可，最后就是镜像的大小有区别。

## 结语

alist官方提供了docker构建脚本，如果自定义的话，不要使用，否则会直接生成官方的docker镜像，而不是自行修改的版本。