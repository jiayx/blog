---
title: Ubuntu搭建匿名socks5代理
categories: 
tags: []
date: 2015-12-08 16:21:00
---

今天刷票，没想到公司ip被12306封了，看到鱼大的刷票软件支持socks5代理，于是想到用服务器搞个代理试试。
步骤如下
1. 安装
apt-get install dante-server
2. 创建日志文件夹
mkdir /var/log/sockd
3. 修改配置文件
vi /etc/danted.conf
写入
```shell
logoutput: /var/log/sockd/sockd.log
internal: 服务器ip port = 1080
external: 服务器ip
method: username none
user.privileged: proxyuser
user.notprivileged: nobody
user.libwrap: nobody
client pass {
    from: 0.0.0.0/0 to: 0.0.0.0/0
    log: connect disconnect
}
pass {
    from: 0.0.0.0/0 to: 0.0.0.0/0 port gt 1023
    command: bind
    log: connect disconnect
}
pass {
    from: 0.0.0.0/0 to: 0.0.0.0/0
    command: connect udpassociate
    log: connect disconnect
}
block {
    from: 0.0.0.0/0 to: 0.0.0.0/0
    log: connect error
}
```
4. /etc/init.d/danted start
5. 监测是否启动成功
netstat -anp | grep 1080

------
有个问题就是端口暴露出来 会被各种人扫到。。所以千万不能一直开着
