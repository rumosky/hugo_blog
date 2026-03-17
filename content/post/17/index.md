---
title: "markdown语法大全"
slug: "17"
date: 2019-01-24T14:32:00+08:00
lastmod: 2019-01-24T14:32:00+08:00
categories: ["杂谈"]
tags: ["markdown"]
draft: false
---

## 简述

Markdown是一种可以使用普通文本编辑器编写的标记语言，通过简单的标记语法，它可以使普通文本内容具有一定的格式。

Markdown的语法简洁明了、学习容易，而且功能比纯文本更强，因此有很多人用它写博客。 （本文只用作介绍，详细语法规则，注意事项以及实例，请访问 [markdown语法](<https://www.yuque.com/rumosky/markdown> "markdown语法")

### 基础语法

markdown基础语法，是markdown最常用的标记。

#### 标题

使用`#`表示标题，一级标题使用一个`#`，二级标题使用两个`##`，以此类推，共有六级标题。

#### 次阶标题

使用`=====`表示高阶标题，使用`-----`表示次阶标题。

#### 目录

使用`[TOC]`生成目录。`[TOC]`会把文章内的#标题元素提取出来，生成目录

#### 引用

使用`>`表示引用，`>>`表示引用里面再套一层引用，依次类推。

#### 粗体

使用`**`或者`__`（两个下划线）表示粗体。

#### 斜体

使用`*`或者`_`（一个下划线）表示斜体。

#### 删除线

要加删除线的文字左右分别用两个`~~`（波浪号）号包起来

#### 分割线

三个或者三个以上的`-`（中划线）或者`*`都可以。

#### 超链接

1，使用`[](link "title")`表示行内链接。其中： `[]`内的内容为要添加链接的文字。 `link`为链接地址。 `title`为显示标题。显示效果为在你将鼠标放到链接上后，会显示一个小框提示，提示的内容就是title里的内容。

2，使用`[][序号]`和`[序号]: link "title"`表示参考链接。其中： 第一个`[]`内的内容为要添加链接的文字。 第二个`[]`内的内容是序号。 `link`为链接地址。 `title`为显示标题。显示效果为在你将鼠标放到链接上后，会显示一个小框提示，提示的内容就是`title`里的内容。

### 进阶语法

markdown进阶语法，是markdown不常用的标记。

#### LaTeX公式

行内公式：使用一个`$`符号引用公式: $E=mc^2$


行间公式：使用两个`$$`符号引用公式: 

$$\sum_{i=1}^n a_i=0$$
$$f(x_1,x_x,\ldots,x_n) = x_1^2 + x_2^2 + \cdots + x_n^2$$
$$\sum^{j-1}_{k=0}{\widehat{\gamma}_{kj} z_k}$$

#### 流程图


```mermaid
flowchart LR
A[Hard] -->|Text| B(Round)
B --> C{Decision}
C -->|One| D[Result 1]
C -->|Two| E[Result 2]
```


#### 时序图


```mermaid
sequenceDiagram
Alice->>John: Hello John, how are you?
loop Healthcheck
    John->>John: Fight against hypochondria
end
Note right of John: Rational thoughts!
John-->>Alice: Great!
John->>Bob: How about you?
Bob-->>John: Jolly good!
```


#### 甘特图


```mermaid
gantt
    section Section
    Completed :done,    des1, 2014-01-06,2014-01-08
    Active        :active,  des2, 2014-01-07, 3d
    Parallel 1   :         des3, after des1, 1d
    Parallel 2   :         des4, after des1, 1d
    Parallel 3   :         des5, after des3, 1d
    Parallel 4   :         des6, after des4, 1d
```

#### 状态图


```mermaid
stateDiagram-v2
[*] --> Still
Still --> [*]
Still --> Moving
Moving --> Still
Moving --> Crash
Crash --> [*]
```


#### 饼图

```mermaid
pie
"Dogs" : 386
"Cats" : 85
"Rats" : 15
```