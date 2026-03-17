---
title: "微信每日定时发送问候dailyGreeting"
slug: "112"
date: 2022-11-03T22:22:00+08:00
lastmod: 2024-10-04T18:43:09+08:00
categories: ["随笔"]
tags: ["python"]
draft: false
---

## 前言

每天给女友发送早安晚安比较繁琐，而且也容易忘记，利用微信公众号可以定时发送模板消息，设定内容，十分方便，特此记录。

起因是看到抖音上有人发的视频，上面是每天定时发送早安。自己也弄了一个，相比于其他的，优势就是支持农历生日计算，并且设置生日的时候只需要写月和日，一次即可，不用一年修改一次生日变量。最后就是增加了星座运势，如果女友比较相信这个，可以参考。

### 步骤

1.首先，个人用户需要使用微信公众测试号才可以发送模板消息，认证过的企业号则不用，[微信测试号](https://mp.weixin.qq.com/debug/cgi-bin/sandboxinfo?action=showinfo&t=sandbox/index)，注册之后替换代码内的微信公众号的appId和appSecret

2.同时，天气部分使用高德API，也可以自行替换其他API，[高德天气API](https://console.amap.com/dev/key/app)

3.测试号需要新增模板消息，本文使用的模板消息如下：

```txt
{{name.DATA}} 今天是{{date.DATA}}，星期{{week.DATA}}，每天都是超级爱你的一天~

{{name.DATA}} 现在在{{province.DATA}}{{city.DATA}} 今天天气{{weather.DATA}}，温度{{temperature.DATA}}摄氏度，空气湿度{{humidity.DATA}}%，{{winddirection.DATA}}风{{windpower.DATA}}级，今天也一定要好好爱护自己哦~ 

今天是我们在一起的第{{togetherDays.DATA}}天，这些日子里都有超级爱你哦~ 

距离{{name.DATA}}的生日还有{{birthDays.DATA}}天，😊

{{sentence.DATA}}
```

效果如下：

![早安](https://cdn.rumosky.com/usr/uploads/2022/12/4203739220.jpg)

```txt
❤️{{name.DATA}}❤️ 

我们已经贴贴{{togetherDays.DATA}}天啦！

{{name.DATA}} 你今天已经很努力了，快睡吧~ 

{{sentenceEn.DATA}} 
{{sentenceZh.DATA}}
```

效果如下：

![晚安](https://cdn.rumosky.com/usr/uploads/2022/12/2436168576.jpg)

```txt
{{name.DATA}} 今天你的幸运颜色是{{luckyColor.DATA}}，幸运数字是{{luckyNumber.DATA}}，速配星座是{{luckyConstellation.DATA}} 

短评：{{shortComment.DATA}} 

综合运势：{{fortuneText.DATA}} 
爱情运势：{{fortuneLove.DATA}} 
事业运势：{{fortuneWork.DATA}}
```

效果如下：

![今日运势](https://cdn.rumosky.com/usr/uploads/2022/12/268079085.png)

> 今日运势如果写了上面的幸运色数字之类的，下面的运势就只能选择性的写三个，再多就显示不下了，但源码已经配置了全部的内容，使用时修改对应字段即可

模板消息配置好之后请替换代码里面的模板消息ID，只能写一个

4.想给谁发送，就需要对方关注测试号，关注之后在用户列表里可以看到微信号，将微信号复制到代码里的用户数组内，可以配置多个

5.先使用`pip install requests`安装模块，若使用农历生日，请安装`zhdate`模块，然后使用`python morning.py`查看运行结果

**注意**：农历和公历生日选一个配置即可，当然，两个生日都过的就当我没说

#### 定时发送

Linux可以使用crontab定时任务进行定时发送

宝塔面板请添加定时任务，shell脚本，内容为

```bash
cd /www/wwwroot/python/
python3 morning.py
```

Linux上默认是没有Python3的，安装方法参考：[bspost cid="29"]

GitHub也可以使用Actions进行自动化配置，配置大致如上，图形界面需要手动添加appid等上述内容

#### 问题

目前发现，在模板消息的开头，不要添加表情，否则会产生颜色错位的BUG，如下图：

![颜色错位](https://cdn.rumosky.com/usr/uploads/2022/12/1466612477.jpg)

## 源码

[dailyGreeting](https://gitee.com/rumosky/dailyGreeting)

### 2023.06.21更新

> 微信自2023年5月4日起，调整了微信公众号模板消息的发送，每个value限制20个字，并且字体颜色也失效了，现在都是默认黑色。官方文档也没有更新，微信也没有发布公告，但是公众号都变了

所以，现在调用接口的情话和励志英语以及星座运势，都无法展示，因为内容太长，原来的代码不变，建议将模板消息修改一下，内容如下：

早安文案：

```txt
Hi {{name.DATA}}~
今天是{{date.DATA}}，星期{{week.DATA}}
天气{{weather.DATA}}，温度{{temperature.DATA}}℃，{{winddirection.DATA}}风{{windpower.DATA}}级
我们已经一起度过了{{togetherDays.DATA}}天，😊
距离{{name.DATA}}生日还有{{birthDays.DATA}}天~
```

晚安文案：

```txt
🕥夜深了，{{name.DATA}}
我们在一起{{togetherDays.DATA}}天了
❗{{nightGreeting.DATA}}
Have a good night，快睡吧🌙
```

其中，晚安的nightGreeting是自己写的一个一句话问候，类似于代码中的nickName。

希望微信官方可以恢复原来的模板消息内容，现在的实在太简陋了。