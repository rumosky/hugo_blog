---
title: "一键获取Windows聚焦壁纸并保存为jpg图片脚本"
slug: "48"
date: 2019-10-18T00:59:00+08:00
lastmod: 2019-10-18T00:59:00+08:00
categories: ["技术"]
tags: ["python", "Windows"]
draft: false
---

## 说明

锁屏界面`Windows10聚焦`所产生的图片很多都是著名摄影作品，很好看，但是`Windows`并不能将其设置为桌面壁纸，所以需要手动找到其保存位置，修改文件后缀名为`jpg`就可以保存为图片了，但是这样很麻烦，时间久了再好看的图片也会看腻，写一个脚本用来及时更新图片。

### 内容

脚本分为两个部分，一个`run.bat`批处理文件，仅有两行代码，另一个是`get_wallpaper.py`的`Python`脚本，是核心代码。

#### 源码

```py
'''
@Email: rumosky@163.com
@Author: rumosky
@Gitee: https://gitee.com/rumosky_admin
@Date: 2019-10-16 17:11:17
@Description: 一键获取Windows聚焦壁纸并保存为jpg图片脚本
'''
import os
import shutil
source = r"C:\Users\rumosky\AppData\Local\Packages\Microsoft.Windows.ContentDeliveryManager_cw5n1h2txyewy\LocalState\Assets"
destination = r"D:\wallpaper"

def get_path(dir):
    for root, dirs, files in os.walk(dir):
        pass
    return root, files
    pass

def get_destination_jpg(dir):
    root, files = get_path(destination)
    destination_jpg = []
    for file in files:
        if(file[-3:] == 'jpg'):
            destination_jpg.append(file)
            pass
        pass
    return destination_jpg
    pass

def find_new_jpg():
    des = get_destination_jpg(destination)
    r, source_files = get_path(source)
    new_jpg = []
    count = 0
    for source_file in source_files:
        if source_file+".jpg" not in des:
            count = count+1
            new_jpg.append(source_file)
            pass
        pass
    return count, new_jpg
    pass

def copy():
    count, new_jpg = find_new_jpg()
    if count > 0:
        for jpg_file in new_jpg:
            source_path = source+'\\'+jpg_file
            destination_path = destination+'\\'+jpg_file+'.jpg'
            shutil.copyfile(source_path, destination_path)
            pass
        pass
    return count, new_jpg
    pass

if __name__ == "__main__":
    count, without = copy()
    new_jpg = []
    print("更新了 "+str(count)+" 张图片")
    for new_jpg in without:
        print(new_jpg+".jpg")
        pass
    pass
```

> `source`后的路径就是Windows10聚焦文件保存的位置，注意修改Users后面的用户名为自己的
> `destination`是图片存放的位置，必须提前建立好该文件夹，否则会出错


#### `run.bat`

```bash
python get_wallpaper.py
pause
```

> `run.bat`可以不用，只需要电脑上有能运行`Python`脚本的软件即可，`run`一下`get_wallpaper.py`就行

### 结果

运行一下run.bat，结果如图：

![run.bat结果](https://cdn.rumosky.com/usr/uploads/2019/10/546003628.png)

**大功告成！**