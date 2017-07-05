---
title: beego框架orm
categories: Golang
tags: []
date: 2016-01-04 20:57:00
---

今天使用beego的orm时遇到一个问题：mysql数据库为datetime格式的字段插入之后，数据库看到的时间会比正常时间靠前8个小时。
这个结果可以猜测到是跟时区设置有关的问题，但是找了半天都没找到问题。。

后来百度半天才发现是orm初始化的时候没有设置时区，然后beego系统默认的时区是UTC，数据库的时区是本地时区，所以时间就早了8小时。配置如下
```go
orm.RegisterDataBase("default", "mysql", "root:root@/goblog?charset=utf8&loc=Asia%2FShanghai", 30)
```
