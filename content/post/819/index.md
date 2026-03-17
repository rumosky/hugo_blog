---
title: "黑神话悟空Intel Arc A770显卡性能测试"
slug: "819"
date: 2024-08-14T23:33:00+08:00
lastmod: 2024-08-14T23:33:00+08:00
categories: ["杂谈"]
tags: ["Windows", "Intel", "Arc A770", "黑神话悟空"]
draft: false
---

## 说明

最近黑神话悟空非常火，正好Steam上可以下载官方的测试工具，来看看硬件可以跑到什么帧数，正好我怕我的A770不行，测试一下。

### 正文

首先，放上我电脑的配置：

```bash
CPU：Intel i5-13490F
主板：华硕TUF GAMING B760M-PLUS WiFi
显卡：蓝戟Intel Arc A770 Photon 16G OC 2400MHz超频版
内存：阿斯加特DDR5 6400 弗雷海力士A-die CL32 16G*2
硬盘：致态TiPlus7100 PCIe 4.0 2TB
CPU散热：利民AX120R SE青春版
电源：长城X7 750W全模金牌
机箱：乔斯伯松果D31标准版
显示器：华为MateView 28寸 4K+
```

1，Xess超采，未开启光追，高画质

![Xess超采，未开启光追，高画质](https://cdn.rumosky.com/usr/uploads/2024/08/2924667222.png)

2，TSR超采，未开启光追，高画质

![TSR超采，未开启光追，高画质](https://cdn.rumosky.com/usr/uploads/2024/08/1559242433.png)

3，FSR超采，未开启光追，高画质

![FSR超采，未开启光追，高画质](https://cdn.rumosky.com/usr/uploads/2024/08/677600540.png)

4，FSR超采，超高光追，高画质

![FSR超采，超高光追，高画质](https://cdn.rumosky.com/usr/uploads/2024/08/2735498158.png)

5，FSR超采，低光追，高画质

![FSR超采，低光追，高画质](https://cdn.rumosky.com/usr/uploads/2024/08/3186021602.png)

6，TSR超采，未开启光追，高画质（电脑满载一小时后测试）

![TSR超采，未开启光追，高画质，电脑满载一小时后测试](https://cdn.rumosky.com/usr/uploads/2024/08/4022464942.png)

## 最后

补充一下，TSR超采是虚幻引擎带的，FSR是AMD显卡开源的，XeSS是Intel显卡开源的，但目前测试下来，在黑神话悟空中，其他条件一致的情况下，TSR>FSR>XeSS；高画质和中画质影响不大；光追对于帧数影响很大，光追分为低，中，超高三档。但实际上开启之后，不知道是不是显卡不行，反正开启之后水面看不清，还不如不开启。

本次测试都是4K+分辨率显示器，分辨率为3840*2560