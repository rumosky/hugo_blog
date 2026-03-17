---
title: "Android Studio报错error:connection timed out"
slug: "49"
date: 2019-10-18T01:02:00+08:00
lastmod: 2019-10-18T01:02:00+08:00
categories: ["技术"]
tags: ["Windows", "android", "maven", "gradle"]
draft: false
---

## 说明

在构建`Android`项目的时候，底部的状态栏总是会显示`connection timed out`或`Could not download xxx`等问题，这是由于国内网络问题导致第三方依赖下载不上，特此记录其解决办法。

### 步骤

补充：关于这个错误，基本上都是因为`gradle`版本的问题，两个原因：

1.`gradle`版本不对， 2.`gradle`版本因为翻墙网速太慢的原因（<10kb/s），一直无法下载下来，或者只下载了一部分。

#### 解决办法

1.查看工程目录下需要的`gradle`版本：本地项目代码文件夹`\gradle\wrapper\gradle-wrapper.properties`，用`notepad++`等文本编辑器打开，最后一行如下所示：

```bash
distributionUrl=https://services.gradle.org/distributions/gradle-5.4.1-all.zip
```

其中`gradle-5.4.1-all`即为版本号。

2.查看编译工程用到的`gradle`版本：目录`C:\Users\xxx\.gradle\wrapper\dists\gradle-x.x.x-all`, 有些人可能有多个版本，`gradle-x.x.x-all`即为版本号。

3.这时你会看到，两个版本号是不一样的（如果一样，就不会出现上述错误了，如果是一样还是有错误，说明没有下载完全，只下载了一部分），请参考以下两个方法解决：

##### 方法1：手动下载

1.前往`gradle`官网下载所需版本：[gradle官网](https://services.gradle.org/distributions/ "gradle官网")

2.复制到用户`gradle`目录中：`C:\Users\xxx\.gradle\wrapper\dists\gradle-5.4.1-all\xxxxxxx`

3.重启Android Studio即可。

##### 方法2：配置Gradle国内镜像源

这里不再赘述，具体步骤请移步：[bspost cid="50"]

##### 方法3：修改版本号

修改此目录：`distributionUrl=https\://services.gradle.org/distributions/gradle-5.4.1-all.zip`的`gradle`版本号为用户目录`C:\Users\xxx\.gradle\wrapper\dists\gradle-x.x.x-all`的gradle版本号，重启`Android Studio`

##### 方法4：使用本地Gradle

前往官网下载好`Gradle`的压缩包

下载完成后找到电脑的一个比较合适的目录将其解压

在 Android Studio 的 Settings 里面设置 Gradle 为使用本地 Gradle 具体路径为`File -> Settings -> Build, Execution, Deployment -> Gradle`

选择`Use local Gradle distribution`然后找到刚刚解压的那个目录即可

更改 build.gradle 里面的 Gradle 版本 打开你新建的项目, 找到`Gradle Scripts`找到`gradle-wrapper.properties`

双击打开该文件, 修改`dependencies`里面的`classpath`, 只需把最后面的 Gradle 版本改成你下载的那个版本即可

比如上面的是 5.4.1 版本的, classpath 就是`'com.android.tools.build:gradle:5.4.1'`