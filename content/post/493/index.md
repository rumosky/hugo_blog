---
title: "elementUI选择框实现全选功能"
slug: "493"
date: 2023-04-29T14:46:00+08:00
lastmod: 2023-04-29T14:46:00+08:00
categories: ["技术"]
tags: ["vue", "elementUI"]
draft: false
---

## 前言

如题，前两天写一个功能的时候需要用到选择框的多选功能，记录一下

### 正文

elementUI的官方选择框是没有全选功能的，整理一下思路，在选择框里添加一个新的选项，全选，点击全选的时候，`isAll`方法用来确定是否点击了全选，`selectClick`方法则用来进行判断，逻辑很简单，代码如下：

对应的element组件：

```html
      <el-form-item label="考核人员" prop="userIds">
        <el-select
          v-model="form.userIds"
          multiple
          filterable
          placeholder="请选择考核人员"
          style="width: 80%"
        >
        <!-- 全选功能 -->
          <el-option
            key="all"
            value="all"
            label="全选"
            @click.native="isAll"
          ></el-option>
          <!-- 选择人员 -->
          <el-option
            v-for="item in userList"
            :key="item.userId"
            :label="item.nickName"
            :value="item.userId"
            @click.native="selectClick"
          >
          </el-option>
        </el-select>
      </el-form-item>
```

实现方法：

```js
    selectClick() {
      if (this.userList.length > this.form.userIds.length && this.form.userIds.includes("all")) {
          this.form.userIds.splice(this.form.userIds.indexOf("all"), 1);
      } else if (this.userList.length === this.form.userIds.length && !this.form.userIds.includes("all")) {
        this.form.userIds = ['all', ...this.form.userIds]
      } else {
        if (this.form.userIds.includes("all")) {
        this.form.userIds.splice(this.form.userIds.indexOf("all"), 1);
      }
      }
    },
    isAll() {
      if (this.form.userIds.includes("all")) {
        this.form.userIds = [
          "all",
          ...this.userList.map((item) => item.userId),
        ];
      } else {
        this.form.userIds = [];
      }
    }
```