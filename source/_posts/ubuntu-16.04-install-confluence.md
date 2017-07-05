---
title: Ubuntu 16.04 安装 confluence
categories: 其他
tags: []
date: 2017-05-24 17:18:00
---

大致流程请看[这里][1]已经非常详细了，感谢原博主的教程和资源。
这里介绍安装和使用中遇到的几个问题

### 启动遇到的问题
如果你的服务器只有 1G 内存，使用默认配置是启动不了的，因为默认配置的内存占用比较大。
首先要修改配置 
vi /opt/atlassian/confluence/bin/setenv.sh
搜索 CATALINA_OPTS 
找到 
```bash
CATALINA_OPTS="$CATALINA_OPTS -Xms1024m -Xmx1024m -XX:MaxPermSize=256m -XX:+UseG1GC"
```
-Xms 表示最低内存限制 -Xmx 表示最大内存限制。改成 
```bash
CATALINA_OPTS="$CATALINA_OPTS -Xms256m -Xmx512m -XX:MaxPermSize=256m -XX:+UseG1GC"
```
就可以勉强跑起来了。建议 1G 内存的服务器就不要装了，跑不动。。
对于这个问题的官方回复请看[这里][2]

### mysql 版本的问题
Ubuntu 16.04 自带的 mysql 是 5.7 了，与这个 confluence 版本不兼容，可以在[这里][3]下载兼容 mysql 5.7 的 jar 包，替换 mysql-connector-java-5.1.39-bin.jar 就可以啦！
对于这个问题的官方回复请看[这里][4]

### 修改 baseUrl
使用 admin 账号在网站的后台管理中修改 baseUrl 并没有什么卵用，点到应用商店还是会提示 baseUrl 不匹配，需要在 /opt/atlassian/confluence/conf/server.xml 中做相应的修改。如果使用 nginx 或者 apache 做代理的话，还需要修改几处地方，具体修改方式查看[这里][5]和[这里][6]


  [1]: http://www.chen-hao.com.cn/%E6%90%AD%E5%BB%BA%E5%85%AC%E5%8F%B8wiki%E7%B3%BB%E7%BB%9F-confluence-%E6%B1%89%E5%8C%96%E7%A0%B4%E8%A7%A3%E7%89%88.html
  [2]: https://confluence.atlassian.com/confkb/how-to-fix-out-of-memory-errors-by-increasing-available-memory-154071.html
  [3]: https://github.com/yurii-github/mysql-connector-j/blob/release/5.1/mysql-connector-java-5.1.39-SNAPSHOT-bin.jar
  [4]: https://confluence.atlassian.com/confkb/confluence-fails-to-start-with-error-unknown-system-variable-storage_engine-using-mysql-5-7-x-789090576.html
  [5]: https://confluence.atlassian.com/conf56/configuring-the-server-base-url-658737242.html
  [6]: https://confluence.atlassian.com/kb/proxying-atlassian-server-applications-with-apache-http-server-mod_proxy_http-806032611.html
