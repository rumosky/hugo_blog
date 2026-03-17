---
title: "CentOS7安装Python3"
slug: "29"
date: 2019-05-22T20:49:00+08:00
lastmod: 2019-05-22T20:49:00+08:00
categories: ["技术"]
tags: ["linux", "python"]
draft: false
---

## 说明

今天需要跑一个脚本，预备放在服务器上，结果发现CentOS上没有Python3

### 补充

Python2年底就要停止更新了，但是Linux底层还是用Python2写的，编译安装Python3很多教程有问题，所以记录一下。

#### 安装步骤

先下载安装包

```bash
wget https://www.python.org/ftp/python/3.7.3/Python-3.7.3.tgz
```

解压安装包

```bash
tar -zxvf Python-3.7.3.tgz
```

切换至Python3目录

```bash
cd Python-3.7.3
```

配置安装目录

```bash
./configure --prefix=/usr/local/python3
```

编译安装

```bash
make && make install
```

最后只要有显示`Successfully installed pip-19.0.3 setuptools-40.8.0`就证明安装好了，但是在此时，`Python3`命令还无法使用，创建软链接就好了

最后创建软链接

```bash
ln -s /usr/local/python3/bin/python3 /usr/bin/python3
ln -s /usr/local/python3/bin/pip3 /usr/bin/pip3
```

> 如果提示下面这个错误

```bash
ln: failed to create symbolic link ‘/usr/bin/python3’: File exists
```

执行

```bash
ln -sf /usr/local/python3/bin/python3 /usr/bin/python3
ln -sf /usr/local/python3/bin/pip3 /usr/bin/pip3
```

现在输入`Python3`即可启动Python3

#### 安装出错解决方案

安装时出现下面错误

```bash
ModuleNotFoundError: No module named '_ctypes'
```

解决办法：执行下面两条命令

```bash
yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel

yum install libffi-devel -y
```

安装时出现下面错误

```bash
zipimport.ZipImportError: can't decompress data; zlib not available
```

解决办法：执行下面的命令

```bash
yum -y install zlib*
```