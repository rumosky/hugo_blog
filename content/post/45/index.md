---
title: "Android Studio设置国内镜像源"
slug: "45"
date: 2019-10-17T00:53:00+08:00
lastmod: 2019-10-17T00:53:00+08:00
categories: ["技术"]
tags: ["Windows", "aliyun", "android", "国内镜像"]
draft: false
---

## 说明

没有科学上网工具，国内无法很好的访问Android资源，特记录国内镜像配置过程。

### 步骤

1.打开`Android Studio`，点击左下角的`configure`，选择`SDK manager`，在左侧的菜单中选择`HTTP Proxy`

![HTTP Proxy](https://cdn.rumosky.com/usr/uploads/2019/10/2620820906.png)

或者在工程界面，依次点击`File -> Settings -> Appearance & behavior -> System settings -> HTTP Proxy`也可

2.在上述红色区域内填写下列国内镜像地址：（任选其一即可）

```txt
# 电子科技大学
mirrors.dormforce.net 端口：80

# 南阳理工学院镜像服务器地址:

mirror.nyist.edu.cn 端口:80

# 中国科学院开源协会镜像站地址:

IPV4/IPV6: mirrors.opencas.cn 端口:80

IPV4/IPV6: mirrors.opencas.org 端口:80

IPV4/IPV6: mirrors.opencas.ac.cn 端口:80

# 上海GDG镜像服务器地址:

sdk.gdgshanghai.com 端口:8000

# 北京化工大学镜像服务器地址:

IPv4: ubuntu.buct.edu.cn 端口:80

IPv4: ubuntu.buct.cn 端口:80

IPv6: ubuntu.buct6.edu.cn 端口:80

# 大连东软信息学院镜像服务器地址:

mirrors.neusoft.edu.cn 端口:80

# 郑州大学开源镜像站:

mirrors.zzu.edu.cn 端口：80

# 腾讯Bugly镜像:

android-mirror.bugly.qq.com 端口:8080
```

3.填写好之后点击`Apply`即可