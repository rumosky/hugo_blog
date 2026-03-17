---
title: "elementUI中Tabs标签页点击事件无效及动态切换分页"
slug: "808"
date: 2024-06-28T17:29:00+08:00
lastmod: 2024-06-28T17:30:33+08:00
categories: ["技术"]
tags: ["nodejs", "elementUI"]
draft: false
---

## 前言

如题，在使用Tabs标签页的时候，切换标签页的方法没有生效，记录一下。

### 正文

官方文档示例的代码如下：

```html
<template>
  <el-tabs v-model="activeName" class="demo-tabs" @tab-click="handleClick">
    <el-tab-pane label="User" name="first">User</el-tab-pane>
    <el-tab-pane label="Config" name="second">Config</el-tab-pane>
    <el-tab-pane label="Role" name="third">Role</el-tab-pane>
    <el-tab-pane label="Task" name="fourth">Task</el-tab-pane>
  </el-tabs>
</template>
<script lang="ts" setup>
import { ref } from 'vue'
import type { TabsPaneContext } from 'element-plus'

const activeName = ref('first')

const handleClick = (tab: TabsPaneContext, event: Event) => {
  console.log(tab, event)
}
</script>

<style>
.demo-tabs > .el-tabs__content {
  padding: 32px;
  color: #6b778c;
  font-size: 32px;
  font-weight: 600;
}
</style>
```

附上页面的效果图：

![Tabs标签页效果图](https://cdn.rumosky.com/usr/uploads/2024/06/1245787704.png)

这个页面上面是一个标签页，下面是对应的表格，底部是一个分页，注意，分页要写在对应的`el-tabs`的外面，否则会导致分页底部被遮住一部分。模板部分代码如下：

```html
<template>
  <div class="app-container home">
    <el-row :gutter="20">
      <el-col :span="24" :xs="24">
        <el-tabs v-model="activeName" @tab-click="handleClick">
          <el-tab-pane label="库存预警" name="first">
            <el-table
              v-loading="loading"
              :data="inventoryWarnList"
              empty-text="暂无数据"
              highlight-current-row
            >
              <el-table-column
                label="序号"
                align="center"
                type="index"
                width="50px"
              />
              <el-table-column
                key="wareHouseNo"
                label="仓库编号"
                align="center"
                prop="wareHouseNo"
              />
              <el-table-column
                key="wareHouseName"
                label="仓库名称"
                align="center"
                prop="wareHouseName"
                :show-overflow-tooltip="true"
              />
              <el-table-column
                key="materialNo"
                label="物料编号"
                align="center"
                prop="materialNo"
                :show-overflow-tooltip="true"
              />

              <el-table-column
                key="materialName"
                label="物料名称"
                align="center"
                prop="materialName"
                :show-overflow-tooltip="true"
              />
              <el-table-column
                key="currentNum"
                label="当前库存"
                align="center"
                prop="currentNum"
                :show-overflow-tooltip="true"
              />
              <el-table-column
                key="inventoryWarningValue"
                label="预警库存"
                align="center"
                prop="inventoryWarningValue"
                :show-overflow-tooltip="true"
              />
            </el-table>
          </el-tab-pane>
          <el-tab-pane label="检验预警" name="second">
            <el-table
              v-loading="loading"
              :data="inspectionWarnList"
              empty-text="暂无数据"
              highlight-current-row
            >
              <el-table-column
                label="序号"
                align="center"
                type="index"
                width="50px"
              />
              <el-table-column
                key="wareHouseNo"
                label="仓库编号"
                align="center"
                prop="wareHouseNo"
                :show-overflow-tooltip="true"
              />
              <el-table-column
                key="wareHouseName"
                label="仓库名称"
                align="center"
                prop="wareHouseName"
                :show-overflow-tooltip="true"
              />
              <el-table-column
                key="materialNo"
                label="物料编号"
                align="center"
                prop="materialNo"
                :show-overflow-tooltip="true"
              />
              <el-table-column
                key="lastInspectionTime"
                label="上次预警时间"
                align="center"
                prop="lastInspectionTime"
                ><template #default="scope">{{
                  formatDate(scope.row.lastInspectionTime)
                }}</template>
              </el-table-column>
              <el-table-column
                key="rfidCode"
                label="标签编码"
                align="center"
                prop="rfidCode"
                :show-overflow-tooltip="true"
              />
            </el-table>
          </el-tab-pane>
        </el-tabs>
        <pagination
          v-show="total > 0"
          v-model:page="queryParams.pageNum"
          v-model:limit="queryParams.pageSize"
          :total="total"
          @pagination="loadCurrentPageData"
        />
      </el-col>
    </el-row>
  </div>
</template>
```

一开始，我自己写的`handleClick`如下：

```javascript
const handleClick = async (tab, event) => {
    loading.value = true;
    try {
        switch (tab.name) {
            case 'first':
                currentLoadDataMethod.value = getInventoryWarnListData;
                break;
            case 'second':
                currentLoadDataMethod.value = getInspectionWarnListData;
                break;
            case 'third':
                currentLoadDataMethod.value = getReturnWarnListData;
                break;
            case 'fourth':
                currentLoadDataMethod.value = getScrapWarnListData;
                break;
        }
        await currentLoadDataMethod.value();
    } finally {
        loading.value = false;
    }
};
```

然后发现切换标签页没有效果，根本没有进入这个switch，于是加了一个调试代码：

```javascript
console.log("Tab clicked", tab, event);
```

把这一行写在方法内部，打印内容如下：

```json
{
    "uid": 1145,
    "slots": {},
    "props": {
        "name": "second",
        "label": "库存预警",
        "closable": false,
        "disabled": false,
        "lazy": false
    },
    "paneName": "second",
    "active": true,
    "index": "1",
    "isClosable": false
}
```

问题找到了，`tab.name`是`undefined`，但`tab.props.name`是正确的。修改代码如下：

```javascript
const handleClick = async (tab, event) => {
    loading.value = true;
    try {
        switch (tab.props.name) {
            case 'first':
                currentLoadDataMethod.value = getInventoryWarnListData;
                break;
            case 'second':
                currentLoadDataMethod.value = getInspectionWarnListData;
                break;
        }
        await currentLoadDataMethod.value(); 
    } finally {
        loading.value = false;
    }
};
```

同时，还有一个问题，本来我是把分页写在每一个标签页内部的，但会导致分页底部的一部分被遮住，无法显示完全，所以需要动态的根据当前标签页调用对应的接口来进行分页。

首先定义一下：`const currentLoadDataMethod = ref(null);`，然后在`pagination`里绑定`loadCurrentPageData`，并且写好对应的方法：

```javascript
const loadCurrentPageData = async () => {
    if (currentLoadDataMethod.value) {
        loading.value = true;
        try {
            await currentLoadDataMethod.value();
        } finally {
            loading.value = false;
        }
    }
};
```

对应的方法如下：

```javascript
async function getInspectionWarnListData() {
    loading.value = true; 
    try {
        const response = await getInspectionWarnList(queryParams.value);
        inspectionWarnList.value = response.rows;
        total.value = response.total;
    } catch (error) {
        console.error('Failed to fetch inspection warn list:', error);
    } finally {
        loading.value = false; 
    }
}

async function getInventoryWarnListData() {
    loading.value = true;
    try {
        const response = await getInventoryWarnList(queryParams.value);
        inventoryWarnList.value = response.rows;
        total.value = response.total;
    } catch (error) {
        console.error('Failed to fetch inventory warn list:', error);
    } finally {
        loading.value = false;
    }
}
```

## 结语

很多人问组件的其他内容，这里补充一下，分页记得声明以下内容：

```javascript
import { getCurrentInstance, ref, reactive, toRefs } from 'vue';

const total = ref(0);
const data = reactive({
    queryParams: {
        pageNum: 1,
        pageSize: 10,
    },
});

const { queryParams } = toRefs(data);
```

这样分页就不会出问题。