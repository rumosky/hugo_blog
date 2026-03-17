---
title: "CentOS7下Git编译安装踩坑记录"
slug: "116"
date: 2022-11-29T10:41:00+08:00
lastmod: 2022-11-29T10:41:00+08:00
categories: ["技术"]
tags: ["linux", "gitea", "git"]
draft: false
---

## 说明

最新安装gitea，CentOS默认的git版本是1.8，无法使用，需要安装2.x版本，踩了一些坑，特此记录。

### 步骤

第一步，先卸载原来的`git`

```bash
yum -y remove git
```

第二步，在官网下载源码

```bash
wget https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.38.1.tar.gz
```

第三步，安装git依赖

```bash
yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel gcc perl-ExtUtils-MakeMaker -y
```

第四步，解压缩并进入文件夹

```bash
tar -zxvf git-2.38.1.tar.gz && cd git-2.38.1.tar.gz
```

第五步，配置git安装路径

```bash
./configure prefix=/usr/local/git/
```

第六步，编译并安装

```bash
make && make install
```

第七步，添加全局环境变量

```bash
vim /etc/profile
```

```bash
export PATH=$PATH:/usr/local/git/bin
```

第八步，生效配置

```bash
source /etc/profile
```

第九步，验证

```bash
git --version
```

至此，如果正常显示对应版本号，则表示安装成功

## 补充

若一开始忘记卸载原来的git，直接进行编译安装，则有可能会导致依然显示git版本为1.8.x

此时，先卸载原来的git

```bash
yum -y remove git
```

卸载完成之后，输入git，会提示`No such file or directory`，但进入目录，依然可以看到`/usr/local/git/bin/`下是有`git`这个可执行文件的

建立软链接

```bash
ln -s /usr/local/git/bin/git /usr/bin/git
```

此时再次执行`git --version`就可以正常显示版本号了