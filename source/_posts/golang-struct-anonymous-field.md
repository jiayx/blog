---
title: golang中struct的匿名字段
categories: Golang
tags: []
date: 2016-01-10 18:41:00
---

go语言中的struct定义方式与C语言类似
```go
type Human struct {
    Name string
    Age int
}
```
同时go语言还支持匿名字段（嵌入字段）的定义
```go
type Student struct {
    Human
    Grade int
}
```
上述代码中的Human就是一个匿名字段，这个功能相当于其他语言中的继承
只要Human字段嵌入到了Student中，那么Student就默认包含了Human的所有字段
可以采用这种方式来赋值
```go
student := new(Student)
student.Name = "小明"
student.Age = 18
```
也可以采用这样的方式
```go
student := new(Student)
human := new(Human)
human.Name = "小明"
human.Age = 18
student.Human = human
```
------
