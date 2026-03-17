---
title: "VScode配置vue2用户代码片段"
slug: "39"
date: 2019-07-25T23:37:00+08:00
lastmod: 2019-07-25T23:37:00+08:00
categories: ["技术"]
tags: ["vscode", "vue"]
draft: false
---

## 介绍

如题，使用VScode新建一个文件的时候，比如 vue 文件，我们需要手动输入以下内容：

```html
<template>
  <div>

  </div>
</template>

<script>
export default {
    name: "component_name",
    data() {
        return {

        }
    },
    created() {

    },

    mounted() {

    },

    methods() {

    },

    destroyed() {

    }
}
</script>

<style scoped>

</style>
```

每次新建都需要这么做，显得很麻烦，所以，我们需要配置一个模板

### 配置步骤

点击顶部菜单，文件-首选项-用户代码片段， 选择新建全局代码片段文件，随便输入一个文件名，比如vue

文件默认内容如下：

```json
{
    // Place your global snippets here. Each snippet is defined under a snippet name and has a scope, prefix, body and 
    // description. Add comma separated ids of the languages where the snippet is applicable in the scope field. If scope 
    // is left empty or omitted, the snippet gets applied to all languages. The prefix is what is 
    // used to trigger the snippet and the body will be expanded and inserted. Possible variables are: 
    // $1, $2 for tab stops, $0 for the final cursor position, and ${1:label}, ${2:another} for placeholders. 
    // Placeholders with the same ids are connected.
    // Example:
    // "Print to console": {
    //  "scope": "javascript,typescript",
    //  "prefix": "log",
    //  "body": [
    //      "console.log('$1');",
    //      "$2"
    //  ],
    //  "description": "Log output to console"
    // }
}
```

#### 格式说明

配置文件需要三部分，`prefix`就是触发此模板文件的命令，`body`是模板内容，`description`就是描述信息

### 配置内容

以 vue 文件为例，配置模板内容如下：

```json
{
	"Create vue template": {
		"prefix": "vue2",
		"body": [
			"<template>",
			"  <div>\n$0",
			"  </div>",
			"</template>\n",
			"<script>",
			"export default {",
			"    name: \"${1:component_name}\",",
			"    data() {",
			"        return {\n",
			"        }",
			"    },",
			"    created() {\n",
			"    },\n",
			"    mounted() {\n",
			"    },\n",
			"    methods() {\n",
			"    },\n",
			"    destroyed() {\n",
			"    }",
			"}",
			"</script>\n",
			"<style scoped>\n",
			"</style>"
		],
		"description": "default vue template"
	}
}
```

**注意**：模板里不能使用tab制表符，否则会报错

此时，新建一个 vue 文件，输入`vue2`，按回车，会自动创建模板

> 其他格式代码参考 vue 语法即可