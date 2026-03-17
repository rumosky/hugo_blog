---
title: "Gradle/Maven配置国内镜像源（以Android Studio为例）"
slug: "50"
date: 2019-10-18T01:04:00+08:00
lastmod: 2019-10-18T01:04:00+08:00
categories: ["技术"]
tags: ["Windows", "aliyun", "android", "maven", "国内镜像", "gradle"]
draft: false
---

## 说明

`Gradle`源在国外，国内构建项目的时候经常报错连接超时，修改国内镜像可以解决。

### 方法

配置方式有仅对单个项目生效和对所有项目生效两种方式

#### 对单个项目生效

1.打开`Android Studio`工程文件，找到`build.gradle`

2.使用文本编辑器打开，默认格式如下：

```json
// Top-level build file where you can add configuration options common to all sub-projects/modules.

buildscript {
    repositories {
        google()
        jcenter()

    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.5.1'

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}

allprojects {
    repositories {
        google()
        jcenter()

    }
}

task clean(type: Delete) {
    delete rootProject.buildDir
}
```

３.修改为以下内容：

```json
// Top-level build file where you can add configuration options common to all sub-projects/modules.

buildscript {
    repositories {
        maven {
            url 'https://maven.aliyun.com/repository/google'
        }
        maven {
            url 'https://maven.aliyun.com/repository/public'
        }
        maven {
            url 'https://maven.aliyun.com/repository/jcenter'
        }
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.5.1'

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}

allprojects {
    repositories {
        maven {
            url 'https://maven.aliyun.com/repository/google'
        }
        maven {
            url 'https://maven.aliyun.com/repository/public'
        }
        maven {
            url 'https://maven.aliyun.com/repository/jcenter'
        }
    }
}

task clean(type: Delete) {
    delete rootProject.buildDir
}
```

#### 对所有项目生效

1.打开系统用户的Gradle配置目录：`C:\Users\xxx\.gradle`

2.新建文件`init.gradle`（注意文件后缀名为`gradle`）

```json
allprojects{
    repositories {
        def ALIYUN_REPOSITORY_URL = 'https://maven.aliyun.com/repository/public'
        def ALIYUN_JCENTER_URL = 'https://maven.aliyun.com/repository/jcenter'
        all { ArtifactRepository repo ->
            if (repo instanceof MavenArtifactRepository){
                def url = repo.url.toString()
                if (url.startsWith('https://repo1.maven.org/maven2')) {
                    project.logger.lifecycle "Repository ${repo.url} replaced by $ALIYUN_REPOSITORY_URL."
                    remove repo
                }
                if (url.startsWith('https://jcenter.bintray.com/')) {
                    project.logger.lifecycle "Repository ${repo.url} replaced by $ALIYUN_JCENTER_URL."
                    remove repo
                }
            }
        }
        maven {
            url ALIYUN_REPOSITORY_URL
            url ALIYUN_JCENTER_URL
        }
    }
}
```