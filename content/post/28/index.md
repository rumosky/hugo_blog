---
title: "Visual Studio Code插件koro1FileHeader使用记录"
slug: "28"
date: 2019-05-15T20:48:00+08:00
lastmod: 2019-05-15T20:48:00+08:00
categories: ["随笔"]
tags: ["Windows", "win11", "win10", "vscode"]
draft: false
---

## 介绍

VS code是一个非常方便的工具，加上免费，所以一直在使用。 本文记录一下此插件的配置过程以及配置文件，方便以后使用。

### 作者信息插件

写代码的时候会习惯性的在头部写上描述信息和作者，但是文件过多的时候就不方便手动写，修改文件之后也不方便手动修改时间。

插件里面有好几个文件头部插入作者信息的插件，经过使用，此插件最方便，功能也最为完善。

地址：[koro1FileHeader](https://github.com/OBKoro1/koro1FileHeader)

#### 配置使用

插件安装之后需要自行添加配置文件，在VS code界面下，依次点击`文件-首选项-设置`，出现下图界面，点击`在setting.json中编辑`


![VS code配置](https://cdn.rumosky.com/usr/uploads/2019/05/3573917085.png)

在文件中，加入以下内容: 

```json
    "fileheader.customMade": {
        "Email": "rumosky@163.com",
        "Author": "rumosky",
        "Gitee": "https://gitee.com/rumosky_admin",
        "Date": "Do not edit", 
        "LastEditors": "rumosky",
        "LastEditTime": "Do not edit"
    },
    "fileheader.cursorMode": {
        "description":"", 
        "param":"",
        "return":""
    },
    "fileheader.configObj": {
        "autoAdd": false,
        "autoAlready": true,
        "createFileTime": true,
        "timeNoDetail": false,
        "annotationStr": {
            "head": "/*", 
            "middle": " * @", 
            "end": " */", 
            "use": true
        }
    }
```

> 以上配置内容作用请自行参考插件wiki
> 不知道为什么此插件的插入行数设置无效