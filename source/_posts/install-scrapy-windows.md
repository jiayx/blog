---
title: windows下安装scrapy
categories: 
tags: []
date: 2016-01-13 10:20:00
---

其实也没啥
安装好python后执行
```shell
pip install scrapy
```
但是这样一般情况下会有一个依赖包安装不好
运行下面这句话就可以了
```shell
easy_install lxml
```
如果运行的时候还是报错的话
可能需要装一下win32api
在[这里][1]


  [1]: http://sourceforge.net/projects/pywin32/files/pywin32/Build%20220/
