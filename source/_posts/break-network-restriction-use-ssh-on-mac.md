---
title: Mac 下使用 ssh 突破公司内网限制
categories: 其他
tags: []
date: 2017-03-15 18:55:00
---

今天入职新公司，但是发现网络访问并不自由。。公司内的 WiFi 想要访问外网，要使用公司的内部代理，这就蛋疼了，shadowsocks 都用不了了。但是我发现 ssh 可以直连前几天买的日本服务器，经龙哥提醒，可以使用 ssh 来做代理从而突破限制。下面就是具体方法了。

其实命令很简单
通过 `ssh -f -NT -D 127.0.0.1:7001 username@host` 在本地开一个端口，然后使用 Proxy SwitchySharp (Chrome 扩展) 设置一个 socks5 代理即可（代理设置为 127.0.0.1:7001）。当然这个有一个前提就是，要把自己电脑的 public_key 放到远程服务器，不然要输入密码，体验就大打折扣了。

对各个参数的解释参考了[这里][1]，在此对博主表示感谢。
> - -f 表示后台执行ssh指令
> - -D 表示通过动态端口转发方式打开 ssh 通道
> - -N 表示只连接远程主机，不打开远程shell
> - -T 表示不为这个连接分配TTY
> - -NT 两个参数可以放在一起用，代表这个SSH连接只用来传数据，不执行远程操作



虽然命令足够简单，但是作为一个懒惰的程序员，怎么可能做重复的事情，所以要写一个 alias 来简化它，直接贴代码。
```shell
# ssh proxy
alias jpproxy='ssh -f -NT -D 127.0.0.1:7001 username@host &> /dev/null'
alias jpproxystop="ps aux | grep username@host | grep -v grep | awk '{print \$2}' | xargs kill"
```
这是我用日本服务器搞的 ssh 代理。
第一个 alias 用来开启代理
第二个 alias 用来关闭代理，$xxx 在 shell 里代表变量，所以要用 \ 转义 $

至此我们已经成功翻过两堵高墙，来到了真正的互联网，顿时感觉整个人都自由了。

但是还有个问题，目前代理并不是全局的，之后要看下怎么搞成全局，因为电脑版微信收到的图片都看不了。。。头像也不出来

---------
更新：
可以通过 proxifier 来指定哪个软件走代理
Google 查的注册码 P427L-9Y552-5433E-8DSR3-58Z68 软件还是有点小贵的...

  [1]: http://www.ruanyifeng.com/blog/2011/12/ssh_port_forwarding.html
