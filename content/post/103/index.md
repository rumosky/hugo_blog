---
title: "Python使用turtle画玫瑰、哆啦A梦、小猪佩奇"
slug: "103"
date: 2022-08-10T22:46:00+08:00
lastmod: 2022-08-10T22:46:00+08:00
categories: ["生活"]
tags: ["python", "turtle"]
draft: false
---

## 说明

前两天无聊，情人节画爱心的时候，就顺便画了玫瑰花、哆啦A梦和佩奇，现在附上源码。

Python3安装turtle请参考：

[bspost cid="104"]

### 源码

玫瑰花

```py
'''
Author: rumosky
Email: rumosky@163.com
Date: 2022-08-10 20:25:24
Description: 玫瑰
'''
from turtle import *
import time
 
setup(1000,800,0,0)
speed(0)
penup()
seth(90)
fd(340)
seth(0)
pendown()
 
speed(5)
begin_fill()
fillcolor('red')
circle(50,30)
 
for i in range(10):
    fd(1)
    left(10)
 
circle(40,40)
 
for i in range(6):
    fd(1)
    left(3)
 
circle(80,40)
 
for i in range(20):
    fd(0.5)
    left(5)
 
circle(80,45)
 
for i in range(10):
    fd(2)
    left(1)
 
circle(80,25)
 
for i in range(20):
    fd(1)
    left(4)
 
circle(50,50)
 
time.sleep(0.1)
 
circle(120,55)
 
speed(0)
 
seth(-90)
fd(70)
 
right(150)
fd(20)
 
left(140)
circle(140,90)
 
left(30)
circle(160,100)
 
left(130)
fd(25)
 
penup()
right(150)
circle(40,80)
pendown()
 
left(115)
fd(60)
 
penup()
left(180)
fd(60)
pendown()
 
end_fill()
 
right(120)
circle(-50,50)
circle(-20,90)
 
speed(1)
fd(75)
 
speed(0)
circle(90,110)
 
penup()
left(162)
fd(185)
left(170)
pendown()
circle(200,10)
circle(100,40)
circle(-52,115)
left(20)
circle(100,20)
circle(300,20)
speed(1)
fd(250)
 
penup()
speed(0)
left(180)
fd(250)
circle(-300,7)
right(80)
circle(200,5)
pendown()
 
left(60)
begin_fill()
fillcolor('green')
circle(-80,100)
right(90)
fd(10)
left(20)
circle(-63,127)
end_fill()
 
penup()
left(50)
fd(20)
left(180)
 
pendown()
circle(200,25)
 
penup()
right(150)
 
fd(180)
 
right(40)
pendown()
begin_fill()
fillcolor('green')
circle(-100,80)
right(150)
fd(10)
left(60)
circle(-80,98)
end_fill()
 
penup()
left(60)
fd(13)
left(180)
 
pendown()
speed(1)
circle(-200,23)
 
exitonclick()

```

#### 效果

![玫瑰花][1]

小猪佩奇

```py
'''
Author: rumosky
Email: rumosky@163.com
Date: 2022-08-10 20:25:24
Description: 小猪佩奇
'''

import turtle as t

t.begin_fill()
t.pensize(4)
t.hideturtle()
t.colormode(255)
t.color((255, 155, 192), "pink")
t.setup(840, 500)
t.speed(10)

# 鼻子
t.pu()
t.goto(-100, 100)
t.pd()
t.seth(-30)
t.begin_fill()
a = 0.4
for i in range(120):
    if 0 <= i < 30 or 60 <= i < 90:
        a = a + 0.08
        t.lt(3)  # 向左转3度
        t.fd(a)  # 向前走a的步长
    else:
        a = a - 0.08
        t.lt(3)
        t.fd(a)
t.end_fill()

t.pu()
t.seth(90)
t.fd(25)
t.seth(0)
t.fd(10)
t.pd()
t.pencolor(255, 155, 192)
t.seth(10)
t.begin_fill()
t.circle(5)
t.color(160, 82, 45)
t.end_fill()

t.pu()
t.seth(0)
t.fd(20)
t.pd()
t.pencolor(255, 155, 192)
t.seth(10)
t.begin_fill()
t.circle(5)
t.color(160, 82, 45)
t.end_fill()

# 头
t.color((255, 155, 192), "pink")
t.pu()
t.seth(90)
t.fd(41)
t.seth(0)
t.fd(0)
t.pd()
t.begin_fill()
t.seth(180)
t.circle(300, -30)
t.circle(100, -60)
t.circle(80, -100)
t.circle(150, -20)
t.circle(60, -95)
t.seth(161)
t.circle(-300, 15)
t.pu()
t.goto(-100, 100)
t.pd()
t.seth(-30)
a = 0.4
for i in range(60):
    if 0 <= i < 30 or 60 <= i < 90:
        a = a + 0.08
        t.lt(3)  # 向左转3度
        t.fd(a)  # 向前走a的步长
    else:
        a = a - 0.08
        t.lt(3)
        t.fd(a)
t.end_fill()

# 耳朵
t.color((255, 155, 192), "pink")
t.pu()
t.seth(90)
t.fd(-7)
t.seth(0)
t.fd(70)
t.pd()
t.begin_fill()
t.seth(100)
t.circle(-50, 50)
t.circle(-10, 120)
t.circle(-50, 54)
t.end_fill()

t.pu()
t.seth(90)
t.fd(-12)
t.seth(0)
t.fd(30)
t.pd()
t.begin_fill()
t.seth(100)
t.circle(-50, 50)
t.circle(-10, 120)
t.circle(-50, 56)
t.end_fill()

# 眼睛
t.color((255, 155, 192), "white")
t.pu()
t.seth(90)
t.fd(-20)
t.seth(0)
t.fd(-95)
t.pd()
t.begin_fill()
t.circle(15)
t.end_fill()

t.color("black")
t.pu()
t.seth(90)
t.fd(12)
t.seth(0)
t.fd(-3)
t.pd()
t.begin_fill()
t.circle(3)
t.end_fill()

t.color((255, 155, 192), "white")
t.pu()
t.seth(90)
t.fd(-25)
t.seth(0)
t.fd(40)
t.pd()
t.begin_fill()
t.circle(15)
t.end_fill()

t.color("black")
t.pu()
t.seth(90)
t.fd(12)
t.seth(0)
t.fd(-3)
t.pd()
t.begin_fill()
t.circle(3)
t.end_fill()

# 腮
t.color((255, 155, 192))
t.pu()
t.seth(90)
t.fd(-95)
t.seth(0)
t.fd(65)
t.pd()
t.begin_fill()
t.circle(30)
t.end_fill()

# 嘴
t.color(239, 69, 19)
t.pu()
t.seth(90)
t.fd(15)
t.seth(0)
t.fd(-100)
t.pd()
t.seth(-80)
t.circle(30, 40)
t.circle(40, 80)

# 身体
t.color("red", (255, 99, 71))
t.pu()
t.seth(90)
t.fd(-20)
t.seth(0)
t.fd(-78)
t.pd()
t.begin_fill()
t.seth(-130)
t.circle(100, 10)
t.circle(300, 30)
t.seth(0)
t.fd(230)
t.seth(90)
t.circle(300, 30)
t.circle(100, 3)
t.color((255, 155, 192), (255, 100, 100))
t.seth(-135)
t.circle(-80, 63)
t.circle(-150, 24)
t.end_fill()

# 手
t.color((255, 155, 192))
t.pu()
t.seth(90)
t.fd(-40)
t.seth(0)
t.fd(-27)
t.pd()
t.seth(-160)
t.circle(300, 15)
t.pu()
t.seth(90)
t.fd(15)
t.seth(0)
t.fd(0)
t.pd()
t.seth(-10)
t.circle(-20, 90)

t.pu()
t.seth(90)
t.fd(30)
t.seth(0)
t.fd(237)
t.pd()
t.seth(-20)
t.circle(-300, 15)
t.pu()
t.seth(90)
t.fd(20)
t.seth(0)
t.fd(0)
t.pd()
t.seth(-170)
t.circle(20, 90)

# 脚
t.pensize(10)
t.color((240, 128, 128))
t.pu()
t.seth(90)
t.fd(-75)
t.seth(0)
t.fd(-180)
t.pd()
t.seth(-90)
t.fd(40)
t.seth(-180)
t.color("black")
t.pensize(15)
t.fd(20)

t.pensize(10)
t.color((240, 128, 128))
t.pu()
t.seth(90)
t.fd(40)
t.seth(0)
t.fd(90)
t.pd()
t.seth(-90)
t.fd(40)
t.seth(-180)
t.color("black")
t.pensize(15)
t.fd(20)

# 尾巴
t.pensize(4)
t.color((255, 155, 192))
t.pu()
t.seth(90)
t.fd(70)
t.seth(0)
t.fd(95)
t.pd()
t.seth(0)
t.circle(70, 20)
t.circle(10, 330)
t.circle(70, 30)
t.end_fill()
t.done()

```

#### 效果

![小猪佩奇][2]

哆啦A梦

```py
'''
Author: rumosky
Email: rumosky@163.com
Date: 2022-08-10 20:25:24
Description: 哆啦A梦
'''
import turtle


def flyTo(x, y):
    turtle.penup()
    turtle.goto(x, y)
    turtle.pendown()


def drawEye():
    turtle.tracer(False)
    a = 2.5
    for i in range(120):
        if 0 <= i < 30 or 60 <= i < 90:
            a -= 0.05
        else:
            a += 0.05
        turtle.left(3)
        turtle.fd(a)
    turtle.tracer(True)


def beard():
    """ 画胡子， 一共六根
    """
    # 左边第一根胡子
    flyTo(-37, 135)
    turtle.seth(165)
    turtle.fd(60)

    # 左边第二根胡子
    flyTo(-37, 125)
    turtle.seth(180)
    turtle.fd(60)

    # 左边第三根胡子
    flyTo(-37, 115)
    turtle.seth(193)
    turtle.fd(60)

    # 右边第一根胡子
    flyTo(37, 135)
    turtle.seth(15)
    turtle.fd(60)

    # 右边第二根胡子
    flyTo(37, 125)
    turtle.seth(0)
    turtle.fd(60)

    # 右边第三根胡子
    flyTo(37, 115)
    turtle.seth(-13)
    turtle.fd(60)


def drawRedScarf():
    """ 画围巾
    """
    turtle.fillcolor("red")  # 填充颜色
    turtle.begin_fill()
    turtle.seth(0)  # 朝向右

    turtle.fd(200)  # 前进10个单位
    turtle.circle(-5, 90)

    turtle.fd(10)
    turtle.circle(-5, 90)

    turtle.fd(207)
    turtle.circle(-5, 90)

    turtle.fd(10)
    turtle.circle(-5, 90)

    turtle.end_fill()


def drawMouse():
    flyTo(5, 148)
    turtle.seth(270)
    turtle.fd(100)
    turtle.seth(0)
    turtle.circle(120, 50)
    turtle.seth(230)
    turtle.circle(-120, 100)


def drawRedNose():
    flyTo(-10, 158)
    turtle.fillcolor("red")  # 填充颜色
    turtle.begin_fill()
    turtle.circle(20)
    turtle.end_fill()


def drawBlackdrawEye():
    turtle.seth(0)
    flyTo(-20, 195)
    turtle.fillcolor("#000000")  # 填充颜色
    turtle.begin_fill()
    turtle.circle(13)
    turtle.end_fill()
    turtle.pensize(6)
    flyTo(20, 205)
    turtle.seth(75)
    turtle.circle(-10, 150)
    turtle.pensize(3)
    flyTo(-17, 200)
    turtle.seth(0)
    turtle.fillcolor("#ffffff")
    turtle.begin_fill()
    turtle.circle(5)
    turtle.end_fill()
    flyTo(0, 0)


def drawFace():
    """
    """
    turtle.forward(183)  # 前行183个单位
    turtle.fillcolor("white")  # 填充颜色为白色
    turtle.begin_fill()  # 开始填充
    turtle.left(45)  # 左转45度
    turtle.circle(120, 100)  # 右边那半边脸
    turtle.seth(90)  # 朝向向上
    drawEye()  # 画右眼睛
    turtle.seth(180)  # 朝向左
    turtle.penup()  # 抬笔
    turtle.fd(60)  # 前行60
    turtle.pendown()  # 落笔
    turtle.seth(90)  # 朝向上
    drawEye()  # 画左眼睛
    turtle.penup()  # 抬笔
    turtle.seth(180)  # 朝向左
    turtle.fd(64)  # 前进64
    turtle.pendown()  # 落笔
    turtle.seth(215)  # 修改朝向
    turtle.circle(120, 100)  # 左边那半边脸
    turtle.end_fill()  #


def drawHead():
    """ 画了一个被切掉下半部分的圆
    """
    turtle.penup()  # 抬笔
    turtle.circle(150, 40)  # 画圆, 半径150，圆周角40
    turtle.pendown()  # 落笔
    turtle.fillcolor("#00a0de")  # 填充色
    turtle.begin_fill()  # 开始填充
    turtle.circle(150, 280)  # 画圆，半径150, 圆周角280
    turtle.end_fill()


def drawAll():
    drawHead()
    drawRedScarf()
    drawFace()
    drawRedNose()
    drawMouse()
    beard()
    flyTo(0, 0)
    turtle.seth(0)
    turtle.penup()
    turtle.circle(150, 50)
    turtle.pendown()
    turtle.seth(30)
    turtle.fd(40)
    turtle.seth(70)
    turtle.circle(-30, 270)
    turtle.fillcolor("#00a0de")
    turtle.begin_fill()
    turtle.seth(230)
    turtle.fd(80)
    turtle.seth(90)
    turtle.circle(1000, 1)
    turtle.seth(-89)
    turtle.circle(-1000, 10)
    turtle.seth(180)
    turtle.fd(70)
    turtle.seth(90)
    turtle.circle(30, 180)
    turtle.seth(180)
    turtle.fd(70)
    turtle.seth(100)
    turtle.circle(-1000, 9)
    turtle.seth(-86)
    turtle.circle(1000, 2)
    turtle.seth(230)
    turtle.fd(40)
    turtle.circle(-30, 230)
    turtle.seth(45)
    turtle.fd(81)
    turtle.seth(0)
    turtle.fd(203)
    turtle.circle(5, 90)
    turtle.fd(10)
    turtle.circle(5, 90)
    turtle.fd(7)
    turtle.seth(40)
    turtle.circle(150, 10)
    turtle.seth(30)
    turtle.fd(40)
    turtle.end_fill()

    # 左手
    turtle.seth(70)
    turtle.fillcolor("#FFFFFF")
    turtle.begin_fill()
    turtle.circle(-30)
    turtle.end_fill()

    # 脚
    flyTo(103.74, -182.59)
    turtle.seth(0)
    turtle.fillcolor("#FFFFFF")
    turtle.begin_fill()
    turtle.fd(15)
    turtle.circle(-15, 180)
    turtle.fd(90)
    turtle.circle(-15, 180)
    turtle.fd(10)
    turtle.end_fill()
    flyTo(-96.26, -182.59)
    turtle.seth(180)
    turtle.fillcolor("#FFFFFF")
    turtle.begin_fill()
    turtle.fd(15)
    turtle.circle(15, 180)
    turtle.fd(90)
    turtle.circle(15, 180)
    turtle.fd(10)
    turtle.end_fill()

    # 右手
    flyTo(-133.97, -91.81)
    turtle.seth(50)
    turtle.fillcolor("#FFFFFF")
    turtle.begin_fill()
    turtle.circle(30)
    turtle.end_fill()

    # 口袋
    flyTo(-103.42, 15.09)
    turtle.seth(0)
    turtle.fd(38)
    turtle.seth(230)
    turtle.begin_fill()
    turtle.circle(90, 260)
    turtle.end_fill()
    flyTo(5, -40)
    turtle.seth(0)
    turtle.fd(70)
    turtle.seth(-90)
    turtle.circle(-70, 180)
    turtle.seth(0)
    turtle.fd(70)

    # 铃铛
    flyTo(-103.42, 15.09)
    turtle.fd(90)
    turtle.seth(70)
    turtle.fillcolor("#ffd200")
    turtle.begin_fill()
    turtle.circle(-20)
    turtle.end_fill()
    turtle.seth(170)
    turtle.fillcolor("#ffd200")
    turtle.begin_fill()
    turtle.circle(-2, 180)
    turtle.seth(10)
    turtle.circle(-100, 22)
    turtle.circle(-2, 180)
    turtle.seth(180 - 10)
    turtle.circle(100, 22)
    turtle.end_fill()
    flyTo(-13.42, 15.09)
    turtle.seth(250)
    turtle.circle(20, 110)
    turtle.seth(90)
    turtle.fd(15)
    turtle.dot(10)
    flyTo(0, -150)
    drawBlackdrawEye()


def main():
    turtle.screensize(800, 6000, "#F0F0F0")
    turtle.pensize(3)
    turtle.speed(9)
    drawAll()


if __name__ == "__main__":
    main()
    turtle.mainloop()

```

#### 效果

![哆啦A梦][3]

## 结语

本文使用Python3，turtle-0.0.2，无法安装turtle请搜索本站相应教程。


  [1]: https://cdn.rumosky.com/usr/uploads/2022/12/1931040065.png
  [2]: https://cdn.rumosky.com/usr/uploads/2022/12/2892422196.png
  [3]: https://cdn.rumosky.com/usr/uploads/2022/12/593642370.png