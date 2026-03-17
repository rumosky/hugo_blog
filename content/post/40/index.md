---
title: "vue封装分页组件"
slug: "40"
date: 2019-07-26T22:33:00+08:00
lastmod: 2019-07-26T22:33:00+08:00
categories: ["技术"]
tags: ["vue", "elementUI"]
draft: false
---

## 介绍

如题，一个vue项目经常要用到分页，所以封装成组件，方便复用

> 此组件很简单，大佬请自行忽略；网上很多都是`vue` + `elementUI`封装，也有的使用了vuex，本项目环境：nodejs版本 -6.9.0，vue版本-2.9.6

[在线预览](https://rumosky.gitee.io/vue-pagination "在线预览")

源码：[vue-pagination](https://gitee.com/rumosky/vue-pagination)

### 效果图

**演示流程：**

1. 克隆项目代码到本地
2. 在项目目录下执行`npm install`
3. 执行`npm run dev`
4. 打开浏览器，输入`http://localhost:8080`即可预览


前几页效果

![前几页](https://cdn.rumosky.com/usr/uploads/2019/07/2149233633.png)

中间几页效果

![中间几页](https://cdn.rumosky.com/usr/uploads/2019/07/2447475182.png)

最后几页效果

![最后几页](https://cdn.rumosky.com/usr/uploads/2019/07/699880021.png)

### 使用

分页组件引用只需在template和script中添加代码

#### template部分

在你想展示分页组件的位置添加：

```html
<pagination :num="num" :limit="limit" @getNew="getNew"></pagination>
```

#### script部分

首先，在`script`中引入项目`pagination.vue`文件，代码：

```js
import pagination from "./pagination.vue"
```

然后`script`的`export default`里面添加下列代码

```js
data() {
     return {
       num: 0,
       limit: 10,
       currentList: []
       }
      },
components: {
    pagination
    },
methods: {
    getNew(value) {
        this.currentList = this.activityList.slice(value, value + this.limit);
        }
       },
mounted() {
    this.getNew(0);
       this.num = this.activityList.length;
      }
```

最后，请修改脚本中`activityList`为你自己需要分页的数组名，同时在`v-for`的位置修改为`v-for="xxx in currentList"`

至此，组件已经引用完毕，具体样式可以根据需要自行修改