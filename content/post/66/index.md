---
title: "Python爬取妹子图（针对某个图册）"
slug: "66"
date: 2020-07-06T22:47:00+08:00
lastmod: 2020-07-06T22:47:00+08:00
categories: ["随笔"]
tags: []
draft: false
---

## 说明

网上大多数的脚本都是随机从首页爬取，这个脚本是自己选择想要的集合，爬取全部内容。

### 详细内容

话不多说，上代码

#### 源码

```py
# 爬取妹子图
import requests
from bs4 import BeautifulSoup
import time
# 得到每个页面的链接
def get_url():
    for i in range(1,43): #这里的范围是你想要爬取的妹子图图册的内容来决定的，1，43是爬取1-42页的图，具体数量请看页码
        url = 'http://www.mzitu.com/194287/' + str(i)  #这个链接就是你要爬取的妹子图图册，选择自己喜欢的
        yield url
# 得到妹子图片的链接
def get_girl_url(url_list):
    for url in url_list:
        headers = {'User-Agent':'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.108 Safari/537.36',
                   'Referer': 'http://wwww.mzitu.com'}
        res = requests.get(url,headers=headers)
        html = res.text
        soup = BeautifulSoup(html,'lxml')
        img_url = soup.find(class_='main-image').find('img').get('src')
        yield img_url
# 存储妹子图片到本地
def save_img(img_url_list):
    for img_url in img_url_list:
        Picreferer = {'User-Agent':'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.108 Safari/537.36',
                      'Referer':'http://www.mzitu.com'}  #加Referer属性是防止盗链图的产生，目的是告诉服务器当前请求是从哪个页面请求过来的
        res = requests.get(img_url,headers=Picreferer)
        html = res.content
        filename =  'D:/meizitu/' + img_url.split('/')[-1] #这里保存文件路径请依据自己的电脑位置来存放
        with open(filename,'wb') as f:
            f.write(html)

print("正在下载中……")
list1 = get_url()
list2 = get_girl_url(list1)
save_img(list2)
print("下载完成！")
```

#### 使用步骤

源码内都有注释，下载的范围请按照要下载的图册的页码来确定，保存的位置一定要有那个目录，这个脚本不会自动创建目录。

##### 更新

之前的源代码报错如下

```py
bs4.FeatureNotFound: Couldn't find a tree builder with the features you requested: lxml. Do you need to install a parser library?
```

解决办法：

```py
# 执行
pip install lxml
BeautifulSoup(html,'html_parser') -> BeautifulSoup(html,'lxml')
```