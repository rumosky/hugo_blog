---
title: "CSS动态获取视口尺寸"
slug: "838"
date: 2024-11-14T10:21:33+08:00
lastmod: 2024-11-14T10:21:33+08:00
categories: ["随笔"]
tags: ["css", "html"]
draft: false
---

## 说明

今天看到一篇CSS动态获取视口尺寸的文章记录一下

### 内容

一般获取视口尺寸，都是用JS实现，CSS也可以动态获取，用了反正切函数和正切函数，转换了一下px，代码如下：

```html
<!DOCTYPE html>
<html lang="zh-CN">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>CSS动态获取视口大小</title>
    <style>
      @property --vw {
        syntax: "<length>";
        inherits: true;
        initial-value: 100vw;
      }
      @property --vh {
        syntax: "<length>";
        inherits: true;
        initial-value: 100vh;
      }
      :root {
        --w: tan(atan2(var(--vw), 1px));
        --h: tan(atan2(var(--vh), 1px));
      }
      body::before {
        content: counter(w) "X" counter(h);
        counter-reset: w var(--w) h var(--h);
        font-size: 150px;
        font-weight: 900;
        position: fixed;
        inset: 0;
        width: fit-content;
        height: fit-content;
        margin: auto;
      }
    </style>
  </head>
  <body></body>
</html>
```

## 结语

比较巧妙，反正我之前没想到过，记录一下