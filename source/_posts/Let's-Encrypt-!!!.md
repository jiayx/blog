---
title: Let's Encrypt !!!
categories: 其他
tags: []
date: 2017-04-25 17:44:00
---

之前搞的免费证书还没过期，但是被[标记为不安全][1]了。
今天下午抽空把网站换了 Let's Encrypt，还是这个靠谱。
记录下几个使用中要注意的点，这里以 Ubuntu + Nginx 为例

### 1. 服务器安装 certbot
可以在[这里][2]查看自己使用的系统和服务器软件对应的安装方式

### 2. 执行 certbot certonly
会看到如下提示
```
How would you like to authenticate with the ACME CA?
-------------------------------------------------------------------------------
1: Place files in webroot directory (webroot)
2: Spin up a temporary webserver (standalone)
-------------------------------------------------------------------------------
Select the appropriate number [1-2] then [enter] (press 'c' to cancel): 
```
这里会让我们选择 1 还是 2 **强烈建议选择 1**
因为选择 2 之后 certbot 会在 80 端口临时启动一个 webserver，但是我们的 nginx/apache 往往会占用 80 端口，需要停掉服务才可以继续，一次也就算了，每三个月要续期一次，续期的时候还要停止服务，不太合适。。
**所以我们选 1**

采用 1 以后需要指定一个文件夹用来存放认证文件，需要提前为要加 https 的域名配置好 nginx，certbot 会访问这个文件来确认这个域名绑定到了当前服务器。
设置方法：
假如我们指定的认证文件存放目录为 /data0/html/certbot
那么就在 nginx 中做如下配置
```
location '/.well-known/acme-challenge' {
    root /data0/html/certbot;
}
```
这段配置可以写在另外的文件中，用到时 include 进来。
之后按照 certbot 的提示操作，看到 Congratulations! 就说明证书生成成功了，记录下证书存放的位置，一会配置 nginx 要用到。

### 3. 配置 https
找到网站的 nginx 配置文件加入一个 server {} 配置如下
```
server {
    listen 443;

    ...

    ssl on;
    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;

    ...
}
```
这里省略掉了与 80 端口相同的配置项，只需将端口改为 443，加入以 ssl 开头的 3 行配置就搞定啦！
当然 example.com 要替换成我们自己域名，这个可以说是最简配置了
更多详细说明可以看[这里][3]

### 4. 自动更新证书
每两个月更新一次并重启 nginx
0 0 1 */2 * /usr/bin/certbot renew >> /var/log/certbot.log && /usr/sbin/nginx -s reload

### 5. 可能遇到的问题
本来配置好了，然后自己清理了一下之前的配置，搞出问题了。。。
之前有个 listen 80 default_server 这样的配置 我又加了个 listen 443 default_server 重启 nginx 之后就不能访问了 会提示 ERR_CONNECTION_CLOSED 或者 ERR_SSL_PROTOCOL_ERROR。。。查了半天才解决这个问题，自己给自己挖坑，醉了。。不过好在涨姿势了。
listen 443 default_server 这种写法是不对的。
由于是泛解析，任意子域名都能访问到网站，https 建立连接是在判断 server_name 之前的，所以采用 https 的访问是无法统一处理的，目前采用的方式是：
在解析的域名中加一条配置
```
if ($host != 'blog.jiayx.net' ) {
    return 301 https://blog.jiayx.net$request_uri;
}
```



  [1]: https://security.googleblog.com/2016/10/distrusting-wosign-and-startcom.html
  [2]: https://certbot.eff.org
  [3]: https://segmentfault.com/a/1190000005142228
