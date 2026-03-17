---
title: "PotPlayer使用real-url观看芜湖大司马直播以及获取实时弹幕"
slug: "73"
date: 2020-11-21T23:35:00+08:00
lastmod: 2020-11-21T23:35:00+08:00
categories: ["随笔"]
tags: ["python", "potplayer"]
draft: false
---

## 开始

最近在浏览器上看直播的时候发现经常出问题，或者嫌弃礼物弹幕入场弹幕等影响直播效果的特效，遂使用本地播放器进行直播观看。特此记录。

### 步骤

本文使用的PotPlayer版本如下

```bash
版本：201021(1.7.21311)
官网：http://potplayer.daum.net
```

除了播放器，还需要使用`real-url`来解析直播源

[real-url解析直播源](https://github.com/wbt5/real-url "real-url解析直播源")

#### 安装播放器

此处省略，直接看下一步

#### 获取直播源

1.依次执行下列命令：

```bash
# 获取代码
git clone https://gitee.com/rumosky_admin/real-url

# 进入目录
cd real-url

# 选择相应直播平台并运行（以斗鱼芜湖大司马为例）
python douyu.py
请输入斗鱼直播间号：
606118
http://tx2play1.douyucdn.cn/live/606118rLJ8S0xYNG.flv?uuid=
```

复制一下解析得到的直播源地址

如下图：

![大司马直播源](https://cdn.rumosky.com/usr/uploads/2020/11/1675845856.png)

2.打开PotPlayer，依次点击`打开 -> 打开链接`，或者用快捷键`Ctrl + U`

3.默认会读取剪切板的内容，或者自行粘贴上一步得到的直播源地址，点击确定即可。

4.效果如下：（默认最高画质）

![大司马直播间](https://cdn.rumosky.com/usr/uploads/2020/11/803326741.png)

### real-url填坑

斗鱼导入的包里面有时会有这个错误

```py
ModuleNotFoundError: No module named 'execjs'
```

执行`pip install execjs`是不行的，需要执行下面的命令即可：

```py
pip install PyExecJS
```

### 获取弹幕（填坑记录）

`real-url`代码下的`danmaku`中的`main.py`可以获取弹幕

在上述目录下执行`python main.py`

一直会报错

```py
ModuleNotFoundError: No module named 'Crypto'
```

网上有两个方法：

1.执行`pip install crypto`，然后修改包的名字为大写的`Crypto` 2\.执行`pip install pycrypto`，这个包我一直没安装上，会报好几百行的错误，实在懒得看。

上述两个方法都不行，会报错，具体就不细说了，正确解决办法，执行

```py
pip install pycryptodome

# 若上面的两个包已经安装过了，请先卸载，再执行上面的命令
pip uninstall crypto
pip uninstall pycrypto
```

若出现报错

```py
ModuleNotFoundError: No module named 'google'
```

先执行`pip install google`，若还是报上述错误，再执行`pip install protobuf`即可

正确运行后，如下所示，会在控制台输出用户名以及相应弹幕：

```bash
PS D:\projects\real-url\danmu> python main.py
请输入直播间地址：
https://www.douyu.com/606118
阿冷O那年幸福：起飞
鞋刷丶：0-10也能开团
马老师的饱皮：6666
追nn：++++
落枫之季：33
YDXTattoo震威堂：一波了
陈子柴：叼的一
夜如此泛滥成灾：经典辅助绕后 打野站后
坐看陡壁翻车：瞎子？ @M16射速快：风筝 不出冰杖 什么理解？
hhwisdom：这不一波
无人理解的大叔：别说了 这局被爆没话讲
哇哇专治不服：3号位是冰杖啊
風守miNado：AD这波就离谱吧
Jasperc164：躺赢
无聊di我：主播mvp了 我的青春结束了
RRRRRR9：老rapper了
兜兜的宝宝：6666666
Rekaftzz：曼？
马老师的饱皮：立功
大陆第一卢西奥：电焊工
小丸子家的阿良：666
吼吼colin：谁还敢说我是孤儿
第一次在：来得太晚 背锅
Jasperc164：躺赢
离雨i814：马老师刚才说的经典话是啥来？
丶歌未央丶：主播太强了
金伦捞德一：可以可以
慕惜弱：这个点三项理应吴迪
骷髅猛c卡：立大功
无声海浪70458：你真的秀
努力努力的小面包：AD闪现追着杀
Jasperc164：躺赢
Conn1KK：再来
唯爱陈莎莎：躺
孤匠123：我觉得可以啊
盘丝洞的大爬爬：还不如让这卡特打中
马老师的饱皮：可以
老汉的军大衣：闪现好了
吃菜只吃花生米：减速不能叠加
yiwang197：谁出冰仗啊，除了大招触发冰仗，维克托自带减速不比冰仗好？？
带血的蚊子酱：有一说一 这波可以的
能抱天下：888888
璃月梦漓音：无意冒犯
```

## 结语

> 目前弹幕还是无法在PotPlayer上加载 但在播放器上看直播确实比较清爽，`real-url`的作者也正在积极将弹幕与直播融合，期待以后可以在本地观看更纯净的直播！