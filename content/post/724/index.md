---
title: "Docker自动更新镜像方法（定时任务或Watchtower）"
slug: "724"
date: 2023-11-22T16:56:00+08:00
lastmod: 2023-11-22T16:56:00+08:00
categories: ["技术"]
tags: ["Apple", "苹果", "Mac", "docker", "Watchtower"]
draft: false
---

## 说明

平时运行docker如果需要更新，则需要先停止原来的容器，更新新的镜像，然后再创建新容器，这样操作虽然不繁琐，但是如果容器过多，还是会很麻烦，本文记录一下如何简化自动更新。

### 步骤

推荐使用Watchtower，这个是一个工具，可以自动监控运行的容器，在有新镜像时自动更新；另一种办法是使用脚本或者宝塔面板上的定时服务，在固定间隔时间自动拉取容器镜像，若有更新，则执行更新。

#### Watchtower

首先，先运行Watchtower，docker命令：

```bash
docker run -d --name watchtower -v /var/run/docker.sock:/var/run/docker.sock containrrr/watchtower
```

或docker-compose.yml

```yml
version: '3'
services:
  watchtower:
    image: containrrr/watchtower
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    restart: always
```

完成之后，再运行其他的docker容器时，添加`--label com.centurylinklabs.watchtower.enable=true`即可，添加了这个标签的容器会被watchtower监控并自动更新，docker-compose格式如下：

```yml
version: '3'
services:
  myapp:
    image: your-image:latest
    labels:
      - com.centurylinklabs.watchtower.enable=true
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    restart: always
    # 其他容器配置...
```

#### 定时任务

这里使用了宝塔面板的定时任务，请自行设置运行时间，shell脚本内容如下：

```bash
#!/bin/bash

# 定义容器名称和镜像名称
container_name="your_container_name"
image_name="your_image_name"

# 检查是否有新的镜像可用
docker pull $image_name > /dev/null 2>&1
if [ $? -eq 0 ]; then
    echo "发现新的镜像，停止原容器并创建新容器..."
    # 停止原容器
    docker stop $container_name > /dev/null 2>&1
    if [ $? -eq 0 ]; then
        echo "原容器已停止"
    else
        echo "停止原容器失败"
        exit 1
    fi

    # 删除原容器
    docker rm $container_name > /dev/null 2>&1
    if [ $? -eq 0 ]; then
        echo "原容器已删除"
    else
        echo "删除原容器失败"
        exit 1
    fi

    # 创建新容器
    docker run -d --name $container_name $image_name
    if [ $? -eq 0 ]; then
        echo "新容器已创建"
    else
        echo "创建新容器失败"
        exit 1
    fi
else
    echo "没有发现新的镜像"
fi

```

docker-compose方式如下：

```bash
#!/bin/bash

# 定义docker-compose文件路径
docker_compose_file="your_docker_compose_file.yml"

# 检查是否有新的镜像可用
docker-compose pull -q
if [ $? -eq 0 ]; then
    echo "发现新的镜像，停止原容器并创建新容器..."
    # 停止和删除原容器
    docker-compose down
    if [ $? -eq 0 ]; then
        echo "原容器已停止并删除"
    else
        echo "停止原容器失败"
        exit 1
    fi

    # 创建新容器
    docker-compose up -d
    if [ $? -eq 0 ]; then
        echo "新容器已创建"
    else
        echo "创建新容器失败"
        exit 1
    fi
else
    echo "没有发现新的镜像"
fi

```

## 最后

这里提示一下，可能会有人认为，Watchtower监控其他容器自动更新，那它本身会不会自动更新，是否也需要添加标签来使得它自身保持自动更新。以下是解释：

Watchtower容器本身不需要添加`com.centurylinklabs.watchtower.enable=true`标签来自动更新。Watchtower容器是用于监视和更新其他容器的。它不会自动更新自己，因为Watchtower容器不会解析或使用标签来触发自身的更新。要确保Watchtower容器自动更新，你需要使用其他机制来监视和更新Watchtower容器。一种常见的方法是使用系统级的自动更新机制，如使用操作系统的包管理器或其他工具来更新Docker镜像和容器。若你需要保持Watchtower更新，则需要用下面的命令来更新Watchtower容器：

```shell
docker-compose pull watchtower
docker-compose up -d watchtower
```

这将拉取最新的Watchtower镜像，并使用新的镜像重新启动Watchtower容器。

总结：Watchtower容器的自动更新是指监视和更新其他容器，而不是更新自身。因此，在使用Watchtower时，你需要定期检查Watchtower容器的版本，并手动更新它以获取最新的功能和修复的漏洞。