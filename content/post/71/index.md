---
title: "腾讯2021届秋招校园招聘前端笔试真题（第二次笔试）"
slug: "71"
date: 2020-11-15T23:29:00+08:00
lastmod: 2020-11-15T23:29:00+08:00
categories: ["随笔"]
tags: ["腾讯"]
draft: false
---

## 说明

如题，本文内容为2021届秋招前端岗位第二次笔试真题。

### 题目

题目总共有5道，均为编程题，每题20分，共计100分

#### 合并两个有序数组

```txt
时间限制：C/C++ 1秒，其他语言2秒
空间限制：C/C++ 262144K，其他语言524288K
64bit IO Format: %IId
语言限制：JavaScript（V8 6.0.0）
```

**题目描述**

将两个有序数组合并为一个新的有序数组并返回。

**输入描述：**

> 一共两行，每行一个数组，数组元素逗号分隔

**输出描述：**

> 一共一行，数组元素逗号分隔

**示例1**

> 输入
> 
> > 1,2,4 
> > 1,3,4
> 
> 输出
> 
> > 1,1,2,3,4,4

#### 对象的扁平化

```txt
时间限制：C/C++ 1秒，其他语言2秒
空间限制：C/C++ 262144K，其他语言524288K
64bit IO Format: %IId
语言限制：JavaScript（V8 6.0.0）
```

**题目描述**

某个监控系统将每一秒的访问量存储在一个对象内，请将这个对象扁平化后输出。 如对象为：{"1":123,"2":234,"8":456}，请将其扁平化为：[123,234,0,0,0,0,0,456]，空隙默认为0

**输入描述：**

> 输入为特定格式对象（json）

**输出描述：**

> 符合题目要求的数组（json）

**示例1**

> 输入
> 
> > {"1":123,"2":234,"8":456}
> 
> 输出
> 
> > [123,234,0,0,0,0,0,456]

#### 中位数

```txt
时间限制：C/C++ 2秒，其他语言4秒
空间限制：C/C++ 262144K，其他语言524288K
64bit IO Format: %IId
```

**题目描述**

给 n 个数字a<sub>1</sub>,a<sub>2</sub>,...,a<sub>n</sub>，其中 n 为偶数.

对于每个数字单独删除a<sub>n</sub>之后，剩下的 n-1 个元素中位数是多少。

**输入描述：**

> 第一行，一个偶数 n(n <= 200000) 
> 第二行，输入 n 个数字，第 i 个数字表示 a<sub>i</sub>，a<sub>i</sub> 在32位整数范围内。

**输出描述：**

> 输出 n 行，第 i 行表示删除 a<sub>i</sub>答案之后，剩下的 n-1 个元素中位数是多少。

**示例1**

> 输入
> 
> > 6 
> > 1 2 3 4 5 6
> 
> 输出
> 
> > 4 
> > 4 
> > 4 
> > 3 
> > 3 
> > 3

#### 日历组件

```txt
时间限制：3秒
空间限制：262144K
64bit IO Format: %IId
```

**题目描述**

本题展示了一个日历组件，界面中存在id=jsContainer的节点A，系统会随机实例化各种Calendar实例，请按照如下要求补充完成Calendar函数。

1、每个月份展示，以星期一为起点

2、对于不需要展示的td节点，请将其内容清空

3、如果为当前日期，请在该td节点上加上class"current"，否则不要保留任何class

4、参数year和month均为正整数

5、month范围为1\~12，包括1和12

6、当前界面中，节点A为系统执行 new Calendar(节点A, 2020, 1)后的展示效果(假定当前日期为2020.01.09)

7、请不要手动修改html和css

8、请不要使用第三方插件

![日历组件](https://cdn.rumosky.com/usr/uploads/2020/11/2922761093.jpg)

```js
function Calendar(container, year, month) {
  this.year = year;
  this.month = month;
  this.html = html;
  this.el = null; //TODO: 创建分页组件根节点
  if (!el) return;
  this.el.className = "calendar";
  this.el.innerHTML = this.html();
  container.appendChild(this.el);

  this.el.addEventListener("click", (event) => {
    var el = event.target;
    var isPre = el.classList.contanins("pre");
    var isNext = el.classList.contanins("next");
    if (!isPre && !isNext) return;
    if (isPre) {
      //TODO: 更新this.month和this.year
    } else {
      //TODO: 更新this.month和this.year
    }
    this.el.innerHTML = this.html();
  });

  function html() {

  }
}
```

#### 数字消除

```txt
时间限制：C/C++ 1秒，其他语言2秒
空间限制：C/C++ 262144K，其他语言524288K
64bit IO Format: %IId
```

**题目描述**

有一个数字构成的字符串，连着两个若相同，则可以消除，消除后前部分和后部分会连在一起变成一个，可以继续进行消除，现在想问你能消除几次？

**输入描述：**

> 第一行输入一个整数T，表示接下来有T组测试数据。 对于每组测试数据，输入一个序列s.
> 1 <= T <= 1000
> 1 <= len(s) <= 1000 
> 0 <= s[i] <= 9

**输出描述：**

> 对于每组数据，输出一个答案代表消除的次数

**示例1**

> 输入
> 
> > 2 
> > 43211234 
> > 101
> 
> 输出
> 
> > 4 
> > 0
> 
> 说明
> 
> > 43211234 => 432234 => 4334 => 44 => 无，总共4次，输出4。101无法消除，输出0

## 最后

日历组件题目相应的html和css无法保存，所以只能如题目下图所示，不过影响不大，知道思路即可，请自行编写代码。