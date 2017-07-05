---
title: 树莓派3上手
categories: 树莓派
tags: [树莓派]
date: 2016-08-15 20:59:00
---

今天树莓派到货了，是个不错的玩具，哈哈~

刚拿到的时候突然感觉无从下手啊。。
研究了一会开始搞了。
首先装系统，去官网下了一个 Ubuntu Meta 系统。[这里下载][1]
然后按照购买时淘宝店老板给的说明书刷了系统，就是把镜像刻录到TF卡中，16G的卡装完系统就剩7-8G了。
装好系统之后开机，一切正常。
开始安装各种软件，这时候问题来了，安装报错了。因为是新装的系统，要先执行 sudo apt-get update 才可以开始执行 sudo apt-get install xx。但是Ubuntu的软件源国内速度太慢了，执行 sudo apt-get update 老半天都没反应。于是想着换一个 163 的源。悲催的是163的源并不适合树莓派的 Ubuntu Meta 系统。几经周折之后，开了树莓派的 WiFi 功能连了公司的 VPN 热点这样才能勉强开始更新。
系统装好了，软件有了就可以开始折腾了~

买树莓派的时候顺便买了几颗 LED 于是开始在网上搜教程打算让 LED 闪几下。
我控制树莓派用的是python的 RPi.GPIO 类库，非常方便。
这里有个概念要说一下，可能刚玩树莓派的搞不太清楚，我也是查了半天加上自己试验才搞明白。
就是这个：GPIO.setmode(GPIO.BOARD)
setmode 的时候有两种可选：
1. GPIO.BOARD
2. GPIO.BCM

这两种的区别就是，在代码中配置引脚的高低电平时。
1是按照图中的 圆圈中的数字来对应的
2是按照图中的 GPIOxx后面的数字来对应的
![树莓派引脚对应图][2]

使用第2种模式的时候要注意，不可以设置非 GPIO 开头引脚的电平，因为他们是树莓派预设好的，强行设置会报错。

剩下的应该水到渠成了，问题不大。
这里贴一个我控制 led 闪烁的 python 代码，网上一搜也不少，大同小异。
```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import RPi.GPIO as GPIO
import time

GPIO.setwarnings(False)
GPIO.setmode(GPIO.BOARD)
GPIO.setup(35, GPIO.OUT)
GPIO.setup(37, GPIO.OUT)

GPIO.output(35, GPIO.LOW)

while True:
    GPIO.output(37, GPIO.HIGH)
    time.sleep(0.2)
    GPIO.output(37, GPIO.LOW)
    time.sleep(0.2)

```

初玩树莓派，如有说的不对的地方还请指正。

  [1]: https://www.raspberrypi.org/downloads/
  [2]: https://blog.jiayx.net/usr/uploads/2016/08/2845264793.png
