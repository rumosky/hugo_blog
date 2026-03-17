---
title: "JMeter压力测试工具初体验"
slug: "67"
date: 2020-07-12T22:56:00+08:00
lastmod: 2020-07-12T22:56:00+08:00
categories: ["杂谈"]
tags: ["JMeter"]
draft: false
---

## 介绍

Apache JMeter是Apache组织开发的基于Java的压力测试工具。

### 安装

1. [JMeter](https://jmeter.apache.org/ "JMeter")
2. 下载适配于当前自己计算机jdk版本的JMeter版本，本文使用的是JMeter 5.3
3. 解压后打开`bin`目录，直接运行`jmeter.bat`即可


#### 补充

很多教程上说需要配置环境变量，但其实不用，为了方便只需要将`jmeter.bat`发送一个快捷方式即可

### 测试

> 本文测试的网站为本站，起始测试并发数为50

添加本次测试计划 （右键-->添加-->Threads（Users）-->线程组）

![添加线程组](https://cdn.rumosky.com/usr/uploads/2020/07/421358404.png)

设置线程数为50 （所谓线程数就是并发用户数）

![设置线程数](https://cdn.rumosky.com/usr/uploads/2020/07/1711750899.png)

**注意**：如果是50并发，线程数请填写51，否则填50的话，测试结果可以看到并发是49

添加协议及相关配置信息

![填写需要测试的网址](https://cdn.rumosky.com/usr/uploads/2020/07/3233848251.png)

为线程添加监听器

![添加监视器](https://cdn.rumosky.com/usr/uploads/2020/07/3597877892.png)

得到测试结果

![测试结果](https://cdn.rumosky.com/usr/uploads/2020/07/736962710.png)

### 生成可视化图标

上述的测试结果可能不太直观，JMeter支持导出成HTML的可视化图表和文本文件

#### 导出HTML结果

在JMeter的bin目录下执行：

```bash
.\jmeter -n -t 测试用例文件保存路径 -e -o 可视化图表文件保存路径
# 例如：
.\jmeter -n -t test/testrumosky50.jmx -e -o test/webreport
```

可视化图表如下：

![测试结果可视化图表](https://cdn.rumosky.com/usr/uploads/2020/07/3021566312.png)

#### 导出文本结果

在JMeter的bin目录下执行：

```bash
.\jmeter -n -t 测试用例文件保存路径 -l 文本结果保存路径
# 例如：
.\jmeter -n -t test/testrumosky50.jmx -l test/result/result.txt
```

上述两个命令可以合并执行，例如：

```bash
.\jmeter -n -t test/testrumosky50.jmx -l test/result/result.txt -e -o test/webreport
```