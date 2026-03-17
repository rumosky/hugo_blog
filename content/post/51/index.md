---
title: "SuperBench.sh 一键测试服务器网速和基本参数"
slug: "51"
date: 2019-10-22T01:06:00+08:00
lastmod: 2019-10-22T01:06:00+08:00
categories: ["技术"]
tags: ["linux"]
draft: false
---

## 说明

最近服务器晚上网速总是很慢，为了排查到底是学校网速问题还是服务器问题，遂寻找测速脚本，特此记录

此脚本使用`speedtest`测试，脚本地址：[script](https://github.com/oooldking/script)

### 使用

此脚本可以检测服务器基本参数，具体内容请看后面的运行截图

执行下列命令进行测试：（任选其一即可）

```bash
wget -qO- --no-check-certificate https://raw.githubusercontent.com/oooldking/script/master/superbench.sh | bash
#或
curl -Lso- -no-check-certificate https://raw.githubusercontent.com/oooldking/script/master/superbench.sh | bash

#备用：更多国内测试节点
wget -qO- --no-check-certificate https://raw.githubusercontent.com/wn789/Superspeed/master/superbench.sh | bash 
#或 
curl -Lso- -no-check-certificate https://raw.githubusercontent.com/wn789/Superspeed/master/superbench.sh | bash

#备用：更多国外测试节点
wget -qO- --no-check-certificate https://raw.githubusercontent.com/wn789/Superspeed/master/superbench_hw.sh | bash 
#或
curl -Lso- -no-check-certificate https://raw.githubusercontent.com/wn789/Superspeed/master/superbench_hw.sh | bash
```

#### 结果

话不多说，如图：

![测试结果](https://cdn.rumosky.com/usr/uploads/2019/10/1941221143.png)

其中，底部还有`speedtest`测试的分享链接可以查看

![speedtest结果](https://cdn.rumosky.com/usr/uploads/2019/10/2491732933.png)