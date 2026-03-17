---
title: "二进制安装和Docker安装Gitea的SSH配置"
slug: "119"
date: 2022-12-01T15:20:00+08:00
lastmod: 2022-12-01T15:20:00+08:00
categories: ["技术"]
tags: ["gitea", "git", "ssh"]
draft: false
---

## 说明

前面两天使用了Gitea，发现使用ssh协议克隆有点问题，特此记录。

### 分析

首先，先说最简单的Docker下安装Gitea的ssh配置，官方文档写的很详细了，具体地址如下：[SSH容器直通][1]

> Since SSH is running inside the container, SSH needs to be passed through from the host to the container if SSH support is desired. One option would be to run the container SSH on a non-standard port (or moving the host port to a non-standard port). Another option which might be more straightforward is for Gitea users to ssh to a Gitea user on the host which will then relay those connections to the docker.
> 由于 SSH 在容器内运行，因此，如果需要 SSH 支持，则需要将 SSH 从主机传递到容器。一种选择是在非标准端口上运行容器 SSH（或将主机端口移至非标准端口）。另一个可能更直接的选择是将 SSH 连接从主机转发到容器。

所以，我推荐前者，我们修改原服务器默认的ssh端口为2222，然后将22端口映射给Gitea容器的ssh端口。这样的话，通过默认的docker compose直接安装就可以使用，无需更多的配置。

后者ssh转发，按照文档的步骤来就没问题，**注意**：建议阅读英文版文档，中文版已经很久没有更新了。

#### 二进制安装方式

二进制安装方式下的ssh配置，基本上默认就可以，如果修改了对应的端口，需要修改默认`/etc/ssh/sshd_config`文件

如果执行`ssh -T username@example.com`显示畅通，但git clone ssh链接失败，则表示ssh配置有问题，注意修改配置即可，百度上有很多相关文章，这里就不再赘述。

补充一点，如果修改了默认端口号，测试时需要执行`ssh -T username@example.com -p端口号`

## 结语

ssh配置还是建议使用docker，二进制的问题实在太多了，排除起来也比较麻烦。

  [1]: https://docs.gitea.io/en-us/install-with-docker/#ssh-container-passthrough