---
title: 奇怪的 bug
categories: 其他
tags: []
date: 2017-06-10 01:46:00
---

ios 9.3.2
header 值带空格
setRequestHeader 时会报错 DOM Exception 12

比如
采用 token 认证方式时 一般会给 Authorization 设置 'Bearer ' + token
如果用户尚未登录，这是 token 值为空 
Authorization 的值就为 'Bearer '
就有可能遇到上述的奇怪问题

出现于稍旧的 safari 内核浏览器
