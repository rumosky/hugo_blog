---
title: "NPM使用淘宝镜像的方法"
slug: "44"
date: 2019-10-16T00:50:00+08:00
lastmod: 2019-10-16T00:50:00+08:00
categories: ["技术"]
tags: ["linux", "Windows", "aliyun", "国内镜像"]
draft: false
---

## 说明

npm源在国外，所以国内访问很慢，甚至很多依赖是无法安装的，所以使用国内淘宝源会方便很多，做个记录。

### 步骤

> 有很多方法来配置npm的registry地址，下面根据不同情境列出几种比较常用的方法。

#### 临时使用

注意：原来的`https://registry.npm.taobao.org`已弃用

```bash
npm --registry https://registry.npmmirror.com install express
```

#### 永久使用

```bash
npm config set registry https://registry.npmmirror.com
```

#### 使用cnpm

```bash
npm install -g cnpm --registry=https://registry.npmmirror.com
```

> `cnpm`是淘宝官方定制的一个命令行工具，支持除了`publish`之外的所有命令，使用时仅需要将`npm`替换为`cnpm`即可，但实际使用情况不佳，经常会出现很多莫名其妙的问题，不太建议使用

#### 恢复默认

```bash
npm config set registry https://registry.npmjs.org
```

### 检测方法

查看源设置（两个命令都可以）

```bash
npm config get registry
npm info express
```