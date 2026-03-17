---
title: "elementUI中upload组件隐藏上传框，限制上传格式和大小，预览"
slug: "803"
date: 2024-06-28T16:57:00+08:00
lastmod: 2024-06-28T17:31:19+08:00
categories: ["技术"]
tags: ["nodejs", "elementUI"]
draft: false
---

## 说明

如题，在用`elementUI`的时候，需要用到`Upload`上传组件，当`list-type`为`picture-card`时，即使写了`limit`为1，但还是会出现一个新的上传框，如下图：

![限制上传数为1](https://cdn.rumosky.com/usr/uploads/2024/06/3520541979.png)

这时，需要隐藏掉后面的上传框，官方没有给方法，下面记录一下。

### 正文

首先，说一下思路，一开始准备用`v-if= fileList.length > 0`来判断这个upload的显示，结果会导致上传后直接隐藏上传框，元素位移了，`v-show`也不行，附上相关代码

模板部分代码：

```html
<el-form-item label="物料图片" prop="photo">
  <el-upload
    ref="uploadRef"
    action="#"
    list-type="picture-card"
    :class="{ disabled: uploadDisabled }"
    :limit="1"
    :http-request="requestUpload"
    :on-remove="handleRemove"
    :before-upload="beforeUpload"
    :on-exceed="handleExceed"
    :on-preview="handlePictureCardPreview"
    :file-list="fileList"
  >
    <el-icon><Plus /></el-icon>
  </el-upload>
  <el-dialog v-model="dialogVisible" :visible.sync="dialogVisible">
    <img :src="dialogImageUrl" alt="物料图片预览" />
  </el-dialog>
</el-form-item>
```

脚本部分：

```javascript
const uploadDisabled = ref(false);
const dialogImageUrl = ref('');
const dialogVisible = ref(false);
const fileList = ref([]);

const beforeUpload = file => {
    const isJPGOrPNG = file.type === 'image/jpeg' || file.type === 'image/png';
    const isLt10M = file.size / 1024 / 1024 < 10;

    if (!isJPGOrPNG) {
        proxy.$message.error('上传图片只能是 JPG 或 PNG 格式!');
    }
    if (!isLt10M) {
        proxy.$message.error('上传图片大小不能超过 10MB!');
    }
    return isJPGOrPNG && isLt10M;
};

const handleRemove = (file, fileList) => {
    uploadDisabled.value = false;
    console.log(file, fileList);
};

const requestUpload = options => {
    const { file, onSuccess, onError } = options;
    const formData = new FormData();
    formData.append('file', file);

    uploadProductPicture(formData)
        .then(response => {
            onSuccess(response);
            form.value.photo = response.imgUrl;
            fileList.value.push({ url: response.imgUrl });
            proxy.$message.success('上传成功');
            uploadDisabled.value = true;
        })
        .catch(error => {
            onError(error);
            proxy.$message.error('上传失败');
        });
};

const handleExceed = (files, fileList) => {
    proxy.$message.error('只能上传一张图片');
};

const handlePictureCardPreview = file => {
    dialogImageUrl.value = file.url;
    console.log(dialogImageUrl.value, '预览图片地址');
    dialogVisible.value = true;
};
```

样式部分代码：

```css
:deep(.disabled .el-upload--picture-card) {
    display: none !important;
}
```

注意，在查看或者编辑按钮里，记得加上`uploadDisabled.value = true;`，否则还是会显示上传框

## 效果

放一下图片看看效果，新增时：

![上传图片](https://cdn.rumosky.com/usr/uploads/2024/06/65441862.png)

上传完成效果：

![上传完成效果](https://cdn.rumosky.com/usr/uploads/2024/06/375024954.png)

现在跟文章开头的图片对比，就少了一个上传框，完美解决。