---
title: nginx 配置问题
categories: PHP
tags: [nginx]
date: 2017-04-25 14:43:04
---

nginx 在执行 `!-e $request_filename` 或者 `try_files $uri $uri/ /index.php$uri` 的时候如果文件所在的文件夹没有 **执行权限** 会导致 nginx 认为文件不存在。

遇到这种问题的时候，先查看文件夹权限。尤其是从别的地方下载的文件或者压缩包里解压出来的文件。
