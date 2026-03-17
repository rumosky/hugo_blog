---
title: "Python2与Python3同时安装以及IDLE和pip共存问题"
slug: "7"
date: 2018-08-04T09:15:00+08:00
lastmod: 2018-08-04T09:15:00+08:00
categories: ["技术"]
tags: ["linux", "python", "Windows"]
draft: false
---

## 同时安装Python2与Python3的方法

### 1.先下载Python2和Python3的安装包。

地址：[Python官网](https://www.python.org/downloads/windows/ "Python官网")

#### 注意：

> Python3下载时有很多版本安装包，建议下载`Windows x86 executable installer`（可执行文件） Python2建议下载`Windows x86 MSI installer`版 下载时会有x86和x86_x64，前者是32位，后者是64位。请根据你自己的系统来安装。

### 2.安装Python2和Python3

建议先安装Python2，之后再安装Python3。

截止发稿，Python2最新正式版是`2.7.15`，Python3最新正式版是`3.7.1`

此处不再赘述安装过程，注意：路径名称不能包含空格和中文，最好在安装选项勾选`add python.exe to path`，其余默认即可。

### 3.添加环境变量

若安装时已按照第二步说明添加了系统变量，请直接看第四步，若不确定，则请打开环境变量查看一下即可

Windows10下配置环境变量：路径`此电脑 -> 属性 -> 高级系统设置 -> 环境变量 -> 系统环境 -> Path -> 编辑`，添加`Python根目录`和`Scripts目录`

### 4.解决Python2与Python3共存以及IDLE冲突

进入Python3根目录，复制python和pythonw，然后将其重命名为python3和pythonw3

同样，对于Python2，复制python和pythonw，然后将其重命名为python2和pythonw2

#### 测试是否共存

（1）打开cmd（命令提示符），输入python2，显示如图：

![python2](https://cdn.rumosky.com/usr/uploads/2018/11/1011934288.png)

（2）在cmd里输入python3，显示如图：

![python3](https://cdn.rumosky.com/usr/uploads/2018/11/3944064759.png)

**若输入Python，则默认使用Python3（就是最后安装的Python版本）**

（3）如果出现这样的提示：xxx不是内部或外部命令，也不是可运行的程序或批处理文件。那说明环境变量没配置正确，文件没有按照第四步复制正确。

- 有的教程上说只复制Python2或者3任意一个的python和pythonw文件即可，但是这样不能解决IDLE无法打开的问题。
- 有的教程上是修改python和pythonw文件名，并不是复制，亲测不行，这样的修改IDLE还是会冲突。
- 同时修改两个版本的python和pythonw可以解决IDLE无法打开的状况。
- 最后安装Python3，就是因为系统会默认添加python为最后安装的版本。在cmd里输入python，显示的是Python3，不是Python2

### 5.解决pip共存问题

Python安装包时需要用pip工具，同时安装两个版本Python之后，pip无法区分版本，解决办法就是重新安装pip

（1）在cmd下执行下面两条命令

```bash
python3 -m pip install --upgrade pip --force-reinstall
python2 -m pip install --upgrade pip --force-reinstall
```

![pip](https://cdn.rumosky.com/usr/uploads/2018/11/3789624334.png)

上图`Cache entry deserialization failed, entry ignored`错误，是因为权限不够产生的，使用管理员权限打开cmd就没问题了。不是安装失败，已成功安装pip。

（2）在cmd下执行下面两条命令查看是否成功

```bash
pip3 -V
pip2 -V
```

![pip-version](https://cdn.rumosky.com/usr/uploads/2018/11/3826678807.png)

（3）以后安装Python包需要使用下面的命令

```bash
pip3 install xxx
pip2 install xxx
```