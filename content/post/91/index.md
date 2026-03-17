---
title: "WordPress博客迁移随机添加浏览数据"
slug: "91"
date: 2021-11-15T16:00:00+08:00
lastmod: 2021-11-15T16:00:00+08:00
categories: ["随笔"]
tags: ["WordPress", "typecho"]
draft: false
---

## 说明

前一阵子从WP迁移回了Typecho，博文的浏览数据没了，随机添加一下

### 正文

其实很简单，只有一句SQL，添加一个随机数，示例如下：

```sql
UPDATE `typecho_contents` SET `views`= ceiling(rand()*5000) WHERE 1;
```

数据量想写多少写多少，但建议适量，否则建站几天时间，访问量几十万也不合适