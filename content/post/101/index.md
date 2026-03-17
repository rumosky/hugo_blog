---
title: "七夕特供，一行代码写爱心~给某些人一点帮助"
slug: "101"
date: 2022-08-04T20:40:00+08:00
lastmod: 2022-08-04T20:40:00+08:00
categories: ["随笔"]
tags: ["python", "七夕"]
draft: false
---

## 说明

七夕到了，有些人没有准备礼物，让我弄个东西，正好想起来以前看到的表白代码，额，姑且叫她表白代码吧，话不多说，正文开始。

### 内容

就随便放几个画图的吧，相应的包需要自行安装，如果要发给别人，建议打包成exe发送，我之前常用pyinstaller打包，对应方法请自行查找。

#### 一行代码画爱心

源码：

```py
print('\n'.join([''.join(['iloverumosky'[(x-y)%8]if((x*0.05)**2+(y*0.1)**2-1)**3-(x*0.05)**2*(y*0.1)**3<=0 else' 'for x in range(-30,30)])for y in range(15,-15,-1)]))
```

效果：

![爱心][1]

#### “脉动”爱心

源码：

```py
'''
Author: rumosky
Email: rumosky@163.com
Date: 2022-08-04 20:34:19
Description: 心电图爱心
'''

import numpy as np
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation
fig, ax = plt.subplots() 
ln, = ax.plot([], [], '-', color='r', lw=1)
time_template = 'rumosky-LOVE = %.1fs'
time_text = ax.text(0.05, 0.9, '', transform=ax.transAxes)

# 祝大家七夕快乐

def init():
    ax.set_xlim(-3, 3)
    ax.set_ylim(-2, 3)
    return ln,


def update(ii):
    xdata, ydata = [], []
    for i in range(0, 183):
        xi = (182-i)/100
        xdata.append(0.01*i-1.82)
        yi = (xi**(2/3))+(0.9*(3.3-xi**2)**0.5)*np.cos(ii*(np.pi)*xi)
        if type(yi) == 'complex':
            yi = np.around(abs(yi), decimals=4)
        yi = np.around(yi, decimals=3)
        ydata.append(yi)

    for i in range(0, 182):
        xi = i/100
        xdata.append(xi)
        yi = (xi**(2/3))+(0.9*(3.3-xi**2)**0.5)*np.cos(ii*(np.pi)*xi)
        if type(yi) == 'complex':
            yi = np.around(abs(yi), decimals=4)
        yi = np.around(yi, decimals=3)
        ydata.append(yi)
    ln.set_data(xdata, ydata)
    time_text.set_text(time_template % (ii))
    return ln,


ani = FuncAnimation(fig, update, np.linspace(
    0, 13.14, 100), init_func=init, interval=100)
ani.save('rumoskylove.gif', writer='imagemagick', fps=60)
plt.show()
```

效果：

![脉动爱心][2]

## 结语

附上我最喜欢的两首词，尤其是第二首，愿有情人终成眷属~

```txt
纤云弄巧，飞星传恨，银汉迢迢暗度。金风玉露一相逢，便胜却人间无数。
柔情似水，佳期如梦，忍顾鹊桥归路。两情若是久长时，又岂在朝朝暮暮。
```

```txt
我住长江头，君住长江尾。 日日思君不见君，共饮长江水。
此水几时休，此恨何时已。只愿君心似我心，定不负相思意。
```


  [1]: https://cdn.rumosky.com/usr/uploads/2022/12/1945263005.png
  [2]: https://cdn.rumosky.com/usr/uploads/2022/12/3178973380.png