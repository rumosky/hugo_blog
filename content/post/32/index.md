---
title: "使用phpmailer发送邮件"
slug: "32"
date: 2019-06-24T21:55:00+08:00
lastmod: 2019-06-24T21:55:00+08:00
categories: ["技术"]
tags: ["linux", "Windows", "php"]
draft: false
---

## 介绍

最近需要用PHP发送邮件，找到了phpmailer这个项目，结合HTML整合富文本编辑器，做了一个简单的demo

源码：[sendmail](https://github.com/rumosky/sendmail)

### 截图

首页样式图

![phpmailer首页](https://cdn.rumosky.com/usr/uploads/2019/06/4240430500.png)

编辑器页面

![编辑页面](https://cdn.rumosky.com/usr/uploads/2019/06/2796921431.png)

发送成功

![发送成功页面](https://cdn.rumosky.com/usr/uploads/2019/06/534374677.png)

#### 部分代码

##### wangeditor配置代码

```js
var E = window.wangEditor;
var editor = new E("#editor");
editor.customConfig.menus = [
  "head", // 标题
  "bold", // 粗体
  "fontSize", // 字号
  "fontName", // 字体
  "italic", // 斜体
  "underline", // 下划线
  "strikeThrough", // 删除线
  "foreColor", // 文字颜色
  "backColor", // 背景颜色
  "link", // 插入链接
  "list", // 列表
  "justify", // 对齐方式
  "quote", // 引用
  "emoticon", // 表情
  "image", // 插入图片
  "table", // 表格
  "video", // 插入视频
  "code", // 插入代码
  "undo", // 撤销
  "redo", // 重复
];
editor.create();

```

##### 邮件发送信息获取代码

```js
$("#senMail").click(function () {
  $(this).attr("disabled", "true"); //只能点击一次
  var content = editor.txt.html(); //获取邮件内容
  var title = $("#title").val(); //获取邮件标题
  var receiver = $("#receiver").val(); //获取收件人地址
  //Ajax POST发送
  $.ajax({
    type: "post",
    url: "send.php",
    data: { content: content, title: title, receiver: receiver },
    dataType: "json",
    success: function (res) {
      alert(res.message);
      setTimeout(function () {
        location.reload();
      }, 500); //发送成功后点击确定0.5秒内刷新页面
    },
    error: function () {
      console.log("请求失败");
    },
  });
});

```

##### phpmailer后台处理邮件信息代码

```php
if (!$mail->send()) {
    echo json_encode(array('status' => 'error', 'message' => $mail->ErrorInfo),true);die();
}else{
    echo json_encode(array('status' => 'success', 'message' => '邮件发送成功'),true);die(); 
}

```