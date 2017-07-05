---
title: elasticsearch使用golang build数据
categories: Golang
tags: []
date: 2016-02-26 20:19:00
---

前期build数据使用php脚本，25000条数据（以下时间均为插入25000条测得）大概要4分钟
为了加快build速度，决定采用并发支持比较好的golang来做
第一个demo使用了官方给出的操作示例，单条循环插入
```go
for _, item := range *items {
        _, err = client.Index().
        Index("twitter").
        Type("tweet").
        BodyJson(item).
        Do()
}
```
改写之后，只开一个线程的话时间缩短到1分30秒
开四个或四个以上线程，时间大约30-40秒
这样做还是不够理想，毕竟我们是google级别的工程师^_^（龙哥说的~~）
然后寻找改进方法。
龙哥提到有一个Bulk方法可以批量插入数据。
一番研究之后，代码片段如下：
```go
bulkService := client.Bulk().Index("test").Type("test")
for _, item := range *items {
    fmt.Println("[thread", i, "] ", item.UniqueId)
    r := elastic.NewBulkIndexRequest().Index("test").Type("test").Doc(item)
    bulkService.Add(r)
    // 大于阀值先存一波
    if bulkService.NumberOfActions() > MaxBulkActions {
        bulkService.Do()
    }
}
// 剩余的存一波
if bulkService.NumberOfActions() > 0 {
    bulkService.Do()
}
```
修改过后经测试，四线程是最优的。插入25000条数据大约只需要4秒钟。
