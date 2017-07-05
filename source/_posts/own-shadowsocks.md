---
title: 搭建自己的 ss 服务
categories: 其他
tags: []
date: 2017-04-11 19:23:00
---

ss 是 shadowsocks 的简称。

这里以 Ubuntu 16.04 为例
1. 买好墙外的服务器
2. 使用 root 用户登录到服务器(如使用非 root 用户，下面的所有命令前加 sudo)
3. apt update && apt upgrade && apt autoremove
4. apt install python-pip
5. pip install --upgrade pip
6. pip install setuptools
7. pip install git+https://github.com/shadowsocks/shadowsocks.git@master
8. vi /etc/shadowsocks.json 写入配置文件 格式如下：
```js
{
    "server": "这台服务器的 IP",
    "server_port": 8388,
    "local_address": "127.0.0.1",
    "local_port": 1080,
    "port_password": {
        "1234": "12345678"
    },
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
```
其中 port_password 字段可以添加多条。前面的数字代表对外提供服务的端口，后面的字符串代表使用前面的端口登录到 ss 的密码。
9. ssserver -c /etc/shadowsocks.json

可以在 .bashrc 中加几个别名，后续操作方便一些
alias ssstart='ssserver -c /etc/shadowsocks.json -d start'
alias ssstop='ssserver -c /etc/shadowsocks.json -d stop'
alias ssrestart='ssserver -c /etc/shadowsocks.json -d restart'


